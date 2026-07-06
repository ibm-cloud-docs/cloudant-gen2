---

copyright:
  years: 2017, 2026
lastupdated: "2026-06-22"

keywords: dbaas data protection, top-tier physical platforms, secure access control, data loss, corruption, byok, encryption

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Securing your data in {{site.data.keyword.cloudant_short_notm}}
{: #securing-your-data-in-cloudant}

## {{site.data.keyword.cloudant_short_notm}} DBaaS data protection and security
{: #ibm-cloudant-dbaas-data-protection-and-security-sd}

Protecting application data for large-scale web and mobile apps can be complex,
especially with distributed and NoSQL databases.

Just as it reduces the effort of maintaining your databases
to keep them running and growing nonstop,
{{site.data.keyword.cloudantfull}} also ensures that your data stays secure and protected.
{: shortdesc}

## Tier one physical platforms
{: #top-tier-physical-platforms-sd}

The {{site.data.keyword.cloudant_short_notm}} DBaaS is
physically hosted on the
{{site.data.keyword.cloud}}.
Therefore,
your data is protected by the network and physical security measures that are employed,
including (but not limited to):


-   Access and identity management.
-   General physical security of data centers and network operations center monitoring.
-   Server hardening.



## Secure access control
{: #secure-access-control-sd}

{{site.data.keyword.cloudant_short_notm}} has a multitude of built-in security features,
for you to control access to data:

| Feature | Description |
|--------|------------|
|Authentication | {{site.data.keyword.cloudant_short_notm}} is accessed by using an HTTPS API. Where the API endpoint requires it, the user is authenticated for every HTTPS request {{site.data.keyword.cloudant_short_notm}} receives. {{site.data.keyword.cloudant_short_notm}} supports IAM access controls. For more information, see the [IAM guide](/docs/cloudant-gen2?topic=cloudant-gen2-managing-access-for-cloudant). |
| Authorization | {{site.data.keyword.cloudant_short_notm}} supports IAM access controls. |
| At-rest encryption | All data that is stored in an {{site.data.keyword.cloudant_short_notm}} instance is encrypted at rest. {{site.data.keyword.cloudant_short_notm}} manages the encryption keys for all environments.   |
| In-flight encryption | All access to {{site.data.keyword.cloudant_short_notm}} is encrypted by using HTTPS. |
| Client-side encryption | Customers can use client-side encryption to ensure that the data protection is controlled by the data owner and the data is never visible to the service provider. |
| TLS | {{site.data.keyword.cloudant_short_notm}} requires the use of TLS 1.2+. {{site.data.keyword.cloudant_short_notm}} strongly recommends that you do not pin certificates in your application. Certificates renew regularly, at least annually, and intermediate and root certificates could change when they do. {{site.data.keyword.cloudant_short_notm}} does not send out notifications before certificate renewals. We recommend that you keep your certificate truststore up to date with the latest root certificates. {{site.data.keyword.cloudant_short_notm}} acquires its certificates from Let's Encrypt. You can find their root certificates on the [Let's Encrypt Chains of Trust](https://letsencrypt.org/certificates/){: external} page. {{site.data.keyword.cloudant_short_notm}} sends a notification if we move to a different certificate authority. |
| Endpoints | All {{site.data.keyword.cloudant_short_notm}} instances are provided with an external URL that is publicly accessible and a VPE URL that is only available on the private network. |
| CORS | Enable CORS support for specific domains by using the {{site.data.keyword.cloudant_short_notm}} Dashboard or API. For more information, see the [CORS documentation](/docs/cloudant-gen2?topic=cloudant-gen2-cross-origin-resource-sharing). |
{: caption="{{site.data.keyword.cloudant_short_notm}} security features" caption-side="top"}



## Protection against data loss or corruption
{: #protection-against-data-loss-or-corruption-sd}

{{site.data.keyword.cloudant_short_notm}} has a number of features
to help you maintain data quality and availability:

| Feature | Description |
|--------|------------|
| Redundant and durable data storage | By default, {{site.data.keyword.cloudant_short_notm}} saves to disk three copies of every document to three different availability zones. Saving the copies ensures that a working failover copy of your data is always available, regardless of failures. |
| Data Replication and export | You can replicate your databases continuously between clusters in different data centers. Another option is to export data from {{site.data.keyword.cloudant_short_notm}} (in JSON format) to other locations or sources (such as your own data center) for added data redundancy. |
{: caption="{{site.data.keyword.cloudant_short_notm}} data quality and availability features" caption-side="top"}

## Deleting your data in {{site.data.keyword.cloudant_short_notm}}
{: #data-delete}

You can delete individual documents in the {{site.data.keyword.cloudant_short_notm}} Dashboard or by using an API. Documents are not technically deleted but instead are compacted.

For more information, see [Deletion of data](/docs/cloudant-gen2?topic=cloudant-gen2-general-data-protection-regulation-gdpr-#deletion-of-data).

To delete a document, follow these steps:

   1. Go to {{site.data.keyword.cloudant_short_notm}} Dashboard.
   2. On the Databases page, click the database that contains the documents that you want to delete.
   3. Click the checkbox next to the documents that you want to delete.
   4. Click **Delete**.

   The document is selected for compaction.

For more information, see [Delete a document](/apidocs/cloudant#deletedocument) in the API Reference documentation.

### Deleting {{site.data.keyword.cloudant_short_notm}} instances
{: #service-delete}

You can delete a database instance in the {{site.data.keyword.cloudant_short_notm}} Dashboard or by using an API.

When an instance is deleted, all data within the database, as well as the account-level information, such as authentication data, is deleted automatically after the 7-day grace period ends. {{site.data.keyword.cloudant_short_notm}} doesn’t hold any contact details for the instances that are created by using the platform. If you have support tickets with {{site.data.keyword.cloudant_short_notm}} where you shared information, such as email addresses, that information isn’t removed by this process.

To delete a database, follow these steps:

   1. Go to {{site.data.keyword.cloudant_short_notm}} Dashboard.
   2. On the Databases page, click **Delete** next to the database you want to delete.
   3. Type in the name of the database you want to delete.
   4. Click **Delete Database**.

   The database is removed from the list of databases.

A database deletion cannot be undone.
{: important}

For more information, see [Delete a database](/apidocs/cloudant#deletedatabase) in the API Reference documentation.

The {{site.data.keyword.cloudant_short_notm}} data retention policy describes how long your data is stored after you delete the service. The data retention policy is included in the {{site.data.keyword.cloudant_short_notm}} service description, which you can find in the {{site.data.keyword.cloud_notm}} Terms and Notices.

### Restoring deleted data for {{site.data.keyword.cloudant_short_notm}}
{: #data-restore}

If you delete your account, you have a 7-day grace period during which you can cancel the request to delete it.

A database deletion cannot be undone.
{: important}
