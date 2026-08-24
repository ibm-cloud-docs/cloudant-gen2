---
copyright:
  years: 2026
lastupdated: "2026-08-24"

keywords: cloudant standard migration, gen1 to gen2, classic infrastructure, vpc, backup and restore, private endpoints, version upgrade

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Migrating {{site.data.keyword.cloudant_short_notm}} Standard from Gen 1 to Gen 2
{: #migrating-cloudant}

[Gen 2]{: tag-purple}

This tutorial shows you how to migrate your {{site.data.keyword.cloudant_short_notm}} Standard deployment from Gen 1 on Classic Infrastructure to Gen 2 on VPC.
{: shortdesc}

Use this tutorial to prepare your network and complete a backup and restore migration for {{site.data.keyword.cloudant_short_notm}} Standard deployments.

This migration topic applies to {{site.data.keyword.cloudant_short_notm}} Standard only.

Migration from Gen 1 to Gen 2 involves changing the URL and credentials that your application will use to connect with your {{site.data.keyword.cloudant_short_notm}} instance. It is not possible to retain your original URL.
{: important}

Complete these steps to complete a backup and restore migration:

* [Before you begin](#prereqs)
* [Step 1: Review the migration path](#migration-paths)
* [Step 2: Confirm version and network readiness](#readiness)
* [Step 3: Create a backup of your Gen 1 deployment](#backup-restore)
* [Step 4: Restore the backup to a new Gen 2 deployment](#restore-gen2)
* [Step 5: Update your applications and validate the migration](#validate-backup-restore)
* [Step 6: Complete post-migration tasks](#post-migration)
{: ui}

## Before you begin
{: #prereqs}

- If you have more than one {{site.data.keyword.cloudant_short_notm}} instance, consider migrating them one at a time, acting on the least critical instance first. This will allow you to test the migration process and ensure that your applications are ready for the migration.
- We will refer to the {{site.data.keyword.cloudant_short_notm}}  instance being migrated as the **source** and the new {{site.data.keyword.cloudant_short_notm}} instance as the **target**.
- Ensure that you have the necessary permissions to perform the migration. You will need to have at least read/checkpoint access to the source {{site.data.keyword.cloudant_short_notm}} instance and manager access to create new target {{site.data.keyword.cloudant_short_notm}} instance and the databases on it.
- Consider when you would like to perform the cutover when your application is reconfigured to point to the new target {{site.data.keyword.cloudant_short_notm}} instance. This might be a time when customer impact is low or when you have specialist staff on hand.

Gen 2 {{site.data.keyword.cloudant_short_notm}} can only use IAM authentication. Your application must be able to use IAM authentication. The official {{site.data.keyword.cloudant_short_notm}} SDKs support IAM authentication. If you are using a custom HTTP client, ensure that it supports IBM IAM authentication.
{: important}


## Step 1: Create target {{site.data.keyword.cloudant_short_notm}} instance
{: #create-target-instance}

- Use the [Getting Started guide](/docs/cloudant-gen2?topic=cloudant-gen2-getting-started-with-cloudant) to create a new {{site.data.keyword.cloudant_short_notm}} instance in the same region as the source instance.
- Use the [Usage And Charges guide](/docs/cloudant-gen2?topic=cloudant-gen2-usage-and-charges) to ensure that you have sufficient provisioned capacity on your target instance to match the source instance.
- Use the [Locating Your Service Credentials guide](/docs/cloudant-gen2?topic=cloudant-gen2-locating-your-service-credentials) to create a set of IAM service credentials to use during the migration process.
- Make a note of the IAM API key and the `url` of the new target {{site.data.keyword.cloudant_short_notm}} instance.

## Step 2: Replication
{: #replication}

For each database in the source instance, create a replication from the source database to a new target database. The replication will copy all documents and design documents from the source database to the target database. We will configure the replication to run in "continuous" mode, so any new changes arriving at the source will be copied to the target indefinitely.

Replications are created by adding a document to the `_replicator` database. The document will look like this:

```json
{
  "_id": "mydb-migration",
  "source": {
    "url": "https://xxx-xxx-xxx-xxxx.cloudant.com/mydb",
    "auth": {
      "iam": {
        "api_key": "xju1...TxuS"
      }
    }
  },
  "target": {
    "url": "https://00000000-0000-0000-0000-00000000.abc.eu-de.cloudant.dataservices.appdomain.cloud/mydb",
    "auth": {
      "iam": {
        "api_key": "UElc7...QIaL01Bjn"
      }
    }
  },
  "create_target": true,
  "continuous": true
}
```
{: codeblock}

The `source` and `target` variables use the same database name (`mydb` in this example) but different URLs. The `create_target` flag is set to `true` to create the target database if it does not exist. The `continuous` flag is set to `true` to run the replication in continuous mode.

If you prefer to use legacy authentication on the source database, your replication document might look like this:

```json
{
  "_id": "mydb-migration",
  "source": "https://USERNAME:PASSWORD@xxx-xxx-xxx-xxxx.cloudant.com/mydb",
  "target": {
    "url": "https://00000000-0000-0000-0000-00000000.abc.eu-de.cloudant.dataservices.appdomain.cloud/mydb",
    "auth": {
      "iam": {
        "api_key": "UElc7...QIaL01Bjn"
      }
    }
  },
  "create_target": true,
  "continuous": true
}
```
{: codeblock}

Create one replication document for each database to be copied.

## Step 3: Monitoring replications
{: #monitoring-replications}

Replications take time to copy every change from the source to target database. The length of time taken will depend on the number of documents in the source database, the size of those documents and how many deleted documents are to be copied.

We can monitor the progress of replications by following the [Replication Scheduler documentation in the Advanced Replication guide](/docs/cloudant-gen2?topic=cloudant-gen2-advanced-replication#the-replication-scheduler). Also, see the [Replication Scheduler blog post in the {{site.data.keyword.cloudant_short_notm}} blog](https://blog.cloudant.com/2024/08/15/Replication-Scheduler.html).

A simple way of determining that replication has the source and target in sync is to [look at the number of documents and deleted documents in the source and target databases](/docs/apis/cloudant/cloudant-gen2#getdatabaseinformation){: external}. If they are equal or very close, the replication is complete.

## Step 4: Monitoring index building
{: #monitoring-index-building}

Copying the data is only half the story. Without fully built secondary indexes, queries directed to target database could be slow or time out. To avoid this, we need to ensure that the secondary indexes are fully built in the target database.

Use the [GET /_active_tasks](https://cloud.ibm.com/docs/apis/cloudant/cloudant-gen2#getactivetasks){: external} endpoint to check the status of the index building process. The response will include a list of tasks that are currently running, including any index-building tasks. An empty response indicates that all indexes are fully built, but if new data is being added to the source, there will inevitably be some tasks listed, which are keeping the target's indexes up to date.

The best way to determine if the indexes are ready is to run a query on the target database and see if it returns results. If the query times out, the indexes are not ready yet. If the query returns results in a timely manner, the indexes are ready and we can proceed with the migration.

## Step 5: Update your applications and validate the migration
{: #validate-cutover}

When the data is transferred and secondary indexes are built, we can update our applications to use the new instance. This can involve updating connection strings and configuring the application to use the target's IAM API key.

Remember that the option to back out of the migration remains: simply reconfigure your application to point to the source instance and cut over at a later time. The continuous replications will keep the source and target in sync, but be aware that any writes that landed in the target instance will only exist there, as there are no replications in the target -> source direction.

## Step 6: Complete post-migration tasks
{: #post-migration}

After you have migrated successfully, we have two further tasks:

1. Stop the migration replications. This is achieved by deleting the replication documents, either programmatically or using the dashboard.
2. Delete the source instance. In the {{site.data.keyword.cloud_notm}} dashboard, locate the source instance and delete it.
