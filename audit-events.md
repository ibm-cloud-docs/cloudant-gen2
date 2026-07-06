---

copyright:
  years: 2019, 2026
lastupdated: "2026-06-22"

keywords: principal, action, resource, timestamp, access audit logs, activity tracker

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Auditing events
{: #at_events}

As a security officer, auditor, or manager, you can use the {{site.data.keyword.atracker_full}} service to track how users and applications interact with the {{site.data.keyword.cloudantfull}} service in {{site.data.keyword.cloud}}.
{: shortdesc}

{{site.data.keyword.atracker_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. You can also be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard. For more information, see the [Getting started tutorial for {{site.data.keyword.atracker_full_notm}}](/docs/cloud-logs?topic=cloud-logs-getting-started){: external}.

## Types of events
{: #at_event_types}

{{site.data.keyword.cloudant_short_notm}} forwards two types of events to {{site.data.keyword.atracker_full_notm}}:

- **Management Events** are administrative events that impact the state of an  {{site.data.keyword.cloudant_short_notm}} instance, such as the following management events:
    - Creating or deleting a database.
  
    - Creating a replication job.
    - Creating an index.
- **Data Events** are all the other events that are involved with interacting with {{site.data.keyword.cloudant_short_notm}}, such as the following events:
    - Reading or writing JSON documents.
    - Reading a list of databases.
    - Viewing monitoring endpoints.
    - Authenticating against the service.

## List of events
{: #at_actions-audit-events}

### Management events
{: #at_actions_management-audit-events}

| Action | Description |
|-------|------------|
| `cloudantnosqldb.account-capacity-dbs.read` | Read the maximum number of databases allowed. |
| `cloudantnosqldb.account-current-dbs.read` | Read the current number of databases. |
| `cloudantnosqldb.account-status.configure` | Set the status of an instance. |
| `cloudantnosqldb.account-status.read` | Get the status of an instance. |
| `cloudantnosqldb.activity-tracker-event-types.read` | Get the configured event types of an instance. |
| `cloudantnosqldb.activity-tracker-event-types.write` | Configure the event types for an instance. |
| `cloudantnosqldb.capacity-throughput.read` | Get the current provisioned throughput capacity settings for an instance. |
| `cloudantnosqldb.capacity-throughput.write` | Set the provisioned throughput capacity settings for an instance. |
| `cloudantnosqldb.csp.capture` | Indicates a Content Security Policy failure when you access the Cloudant Dashboard. |
| `cloudantnosqldb.database.create` | Create a database. |
| `cloudantnosqldb.database.delete` | Delete a database. |
| `cloudantnosqldb.ibm-cloud-account-status.configure` | Set the status of an instance. |
| `cloudantnosqldb.ibm-cloud-account-status.read` | Get the status of an instance. |
| `cloudantnosqldb.replicator-database.create` | Create `_replicator` database. |
| `cloudantnosqldb.replicator-database.delete` | Delete `_replicator` database. |
| `cloudantnosqldb.users-database.create` | Create `_users` database. |
| `cloudantnosqldb.users-database.delete` | Delete `_users` database. |
| `cloudantnosqldb.database-security.read` | Read a security document. |
|`cloudantnosqldb.database-security.write` | A create, update, or delete of a security document. |
| `cloudantnosqldb.design-document.write` | A create, update, or delete of a `_design` document. |
| `cloudantnosqldb.replication.read` | Read a replication document. |
| `cloudantnosqldb.replication.write` | A create, update, or delete of a replication document. |
| `cloudantnosqldb.users.write` | Create, update, or delete a `_users` document. |
{: caption="Management actions" caption-side="top"}



### Data events
{: #at_actions_data-audit-events}

| Action | Description |
|-------|------------|
| `cloudantnosqldb.any-document.read` | Read a JSON document. |
| `cloudantnosqldb.account-all-dbs.read` | Read a list of all databases. |
| `cloudantnosqldb.account-dbs-info.read` | Read metadata about a database. |
| `cloudantnosqldb.account-search-analyze.execute` | Read search index statistics and size. |
| `cloudantnosqldb.account-uuids.read` | Read `_uuids` endpoint. |
| `cloudantnosqldb.account-active-tasks.read` | Read `_active_tasks`. |
| `cloudantnosqldb.current-throughput.read` | Get the current consumption of the provisioned throughput capacity for an instance. |
| `cloudantnosqldb.database-ensure-full-commit.execute` | Post to `_ensure_full_commit` endpoint. |
| `cloudantnosqldb.database-info.read` | Read database metadata. |
| `cloudantnosqldb.data-document.write` | Write a JSON document. |
| `cloudantnosqldb.iam-session.read` | Read IAM session. |
| `cloudantnosqldb.iam-session.write` | Write IAM session. |
| `cloudantnosqldb.iam-session.delete` | Delete IAM session. |
| `cloudantnosqldb.ibmid-login.authenticate` | Complete IAM authentication on the Cloudant Dashboard. |
| `cloudantnosqldb.ibmid-login.receive` | Part of the Cloudant Dashboard login with IAM authentication. |
| `cloudantnosqldb.ibmid-login.start` | Initiate Cloudant Dashboard login with IAM authentication. |
| `cloudantnosqldb.local-document.write` | Write a `_local` document. |
| `cloudantnosqldb.replicator-database-info.read` | Read `_replicator` database information.|
| `cloudantnosqldb,replicator-design-document.write` | Write a `_design` document to the `_replicator` database. |
| `cloudantnosqldb.replicator-local-document.write` | Write a `_local` document to the `_replicator` database. |
| `cloudantnosqldb.sapi.lastactivity` | Get the last active time of an instance. Used internally by the IBM Cloud platform. |
| `cloudantnosqldb.sapi.supportattachments` | Attach file to support ticket. |
| `cloudantnosqldb.sapi.supporttickets` | Create, read, and delete support tickets. |
| `cloudantnosqldb.sapi.usage-data-volume` | Get the data usage of an instance. |
| `cloudantnosqldb.sapi.userccmdiagnostics` | Get history of throughput consumption and 429 requests for the past 5 seconds. |
| `cloudantnosqldb.users.read` | Read `_users` database documents. |
| `cloudantnosqldb.users-database-info.read` | Read `_users` database information. |
| `cloudantnosqldb.users-design-document.write` | Write a `_design` document. |
| `cloudantnosqldb.users-local-document.write` | Write a `_local` document to the `_users` database. |
{: caption="Data actions" caption-side="top"}



## Viewing events
{: #at_ui_ma}

Management events generated by an instance of the {{site.data.keyword.cloudant_short_notm}} service are automatically collected and forwarded to the {{site.data.keyword.atracker_full_notm}} service. You can route auditing events in your account to destinations of your choice by configuring targets and routes, which determine where activity tracking events are delivered. One common target is {{site.data.keyword.logs_full_notm}}, where you can view audit logs, set up monitoring, and configure alerts to track important changes and behaviors.
