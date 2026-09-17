---

copyright:
  years: 2026
lastupdated: "2026-09-03"

keywords: tombstone, delete

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Automatic cleanup of deleted documents (tombstones)
{: #faq-enabling-automatic-removal-of-document-tombstones}
{: faq}
{: support}

{{site.data.keyword.cloudant_short_notm}} now removes retained internal metadata about deleted documents (called tombstones) after 90 days to improve the performance of index building, replication, and changes feed followers.
{: shortdesc}

## What is {{site.data.keyword.cloudant_short_notm}} changing?
{: #what-is-cloudant-changing}
{: faq}

Deleted documents in {{site.data.keyword.cloudant_short_notm}} leave behind a small data structure known as a "tombstone", to mark the place where the document previously existed. The tombstone information is used during replication to replicate details of deleted documents. Other than this, the information serves no purpose.

Previous versions of {{site.data.keyword.cloudant_short_notm}} retained tombstone data forever.

Retaining this metadata indefinitely could cause performance impacts when the numbers of tombstones greatly outnumber the live documents in a single database. The purpose of this change is to remove this problem.

{{site.data.keyword.cloudant_short_notm}} now guarantees that deleted document metadata remains for a minimum of **90 days**, after which time it is completely removed from {{site.data.keyword.cloudant_short_notm}}. The timing of removal isn't exact — it may take place a few days after the 90 day cutoff.

**Most likely impact**: replications and `_changes` consumers offline beyond 90 days will permanently miss some deletes. Read on for more guidance on mitigating this impact.

## What is the roll-out schedule?
{: #roll-out-schedule}
{: faq}

{{site.data.keyword.cloudant_short_notm}} will begin automatic clean up in the second half of 2026.

After the feature is enabled, all metadata for documents that were fully deleted more than 90 days ago will start to be removed. To avoid excessive resource use, we will throttle removal speed and so it will be some weeks before we complete the initial clean up and enter a steady state.

## What documents will be affected by the change?
{: #what-documents-are-affected}
{: faq}

Only metadata about _fully deleted_ documents will be removed.

A high-level definition of _fully deleted_ is: if a `GET` request for the document returns `404`, it is _fully deleted_.

For most users, this simple rule suffices. For advanced cases involving conflicts, here's how it works under the hood.

Documents can have multiple internal branches due to replication conflicts. During [conflict resolution](/docs/cloudant-gen2?topic=cloudant-gen2-conflicts#how-to-resolve-conflicts), each deleted branch is marked with _tombstones_, leaving a single _live_ branch.

Knowing this, here's the precise definition for advanced users: a document is _fully deleted_ only when there are no remaining live branches.

## When does the 90-day retention period start?
{: #when-does-retention-period-start}
{: faq}

It is measured from the HTTP `DELETE` operation that removes the last live branch of a document. In the absence of conflicts, documents have a single branch.

For documents with multiple branches or conflicts, the retention period starts from the write that removes the last live branch, making the document fully deleted. Conflict resolution that leaves at least one live branch does **not** start the retention period; only when all branches are deleted does the timer begin.

## How long after 90 days will removal actually occur?
{: #how-long-after-90-days}
{: faq}

Typically, they are removed between one and seven days after the 90 day cutoff.

## Can customers opt out?
{: #can-customers-opt-out}
{: faq}

Customers cannot opt out of this automatic cleanup.

## How does this affect audit and compliance needs?
{: #audit-and-compliance}
{: faq}

Customers relying on tombstones as a way to record deletion of data should instead use {{site.data.keyword.cloudant_short_notm}}'s [audit log capabilities](/docs/cloudant-gen2?topic=cloudant-gen2-audit-logging) to record this information. This requires audit logs to be configured to emit **Data events**, which is not the default configuration.

## What are tombstones?
{: #what-are-tombstones}
{: faq}

Tombstones are small pieces of metadata that are used to mark branches in {{site.data.keyword.cloudant_short_notm}}'s internal document tree as deleted. They are a special document revision that exists at the end of the branch that contains no data. They take up <100 bytes of storage space.

The cleanup process involves completely removing these tombstone document revisions and all other metadata {{site.data.keyword.cloudant_short_notm}} stores about the document.

While discouraged, the {{site.data.keyword.cloudant_short_notm}} API does allow a user to store data in deleted documents. This data will also be removed by the cleanup process. There is more detail below on how to mitigate this problem if you are affected.

## Why is {{site.data.keyword.cloudant_short_notm}} starting to automatically remove tombstones?
{: #why-remove-tombstones}
{: faq}

{{site.data.keyword.cloudant_short_notm}} databases with a high level of document turnover can suffer from performance impacts when the number of deletions becomes sufficiently high.

Most customers don't need to know about tombstones. They do their job of replicating deletes between databases behind the scenes because there is no need to worry about them in the majority of cases.

In our experience, the only times customers need to know about tombstones is when tombstones are causing issues with a database. The cause of this is always that we have a build up of tombstones, sometimes over many years, that are now a problem.

While this only affects a small number of customers, the only way to fix the problem previously was to create a new database and use filtered replication to only copy live documents to a new database.

## When do tombstones become a problem for databases?
{: #when-do-tombstones-become-a-problem}
{: faq}

Tombstones are very small documents, so you need a lot of them to cause problems. They start to become a problem when your database contains several tens or hundreds of millions of tombstones.

## Potential customer impacts
{: #potential-customer-impacts}
{: faq}

There are three scenarios which may be impacted by this change:

1. Replications that are often offline for extended periods.
1. Applications following `_changes` that are often offline for extended periods.
1. Applications which use specific APIs to store information in deleted documents (a very unusual pattern).

### How does this affect replications?
{: #impact-on-replications}
{: faq}

Because the metadata records the deletion event, after it is removed the deletion event is no longer stored in the database.

For replications, this means that if a replication is resumed after the metadata has been removed, that replication will never learn of the delete, and so it will not be replicated.

If it is essential to see all deletes, which it almost always is, replications must be run more often than every 90 days. This ensures that they process all changes and record a new checkpoint document to resume from that is newer than 90 days old.

### How does this affect applications using `_changes` for change data capture?
{: #impact-on-changes-followers}
{: faq}

Similarly to replications, if a changes follower is resumed after retained deletion metadata has been removed (that is, after 90 days + removal window) then the change data capture application will never see that delete.

If it is essential to see all deletes, applications using `_changes` must ensure that they read and process changes, and store their updated `_changes` sequence value, more often than every 90 days.

Applications may want to consider alerts for extended outages if it is possible they will cross the retention threshold.

### How does this affect customers that store data in tombstones?
{: #impact-on-data-in-tombstones}
{: faq}

We are not aware of any customers using {{site.data.keyword.cloudant_short_notm}} in the way detailed below, but are listing this scenario for completeness.

The safe way to delete {{site.data.keyword.cloudant_short_notm}} documents is by using the HTTP `DELETE` verb. However, using specific API calls, it is possible for customers to store data in tombstone document revisions. While these documents are not retrievable using common APIs, they can be retrieved if their ID and revision ID are known. This usage is strongly discouraged, and we have only seen it used very rarely.

**Fully deleted documents that have had user data added to tombstone records will still be cleaned up by this process and the data will be lost.**

If you use this approach and wish to retain the data currently in tombstone revisions, you will need to:

1. Update your data model to use a _soft-delete_ approach. This is done in two main ways:
    - A user-field (i.e. `deleted` rather than the reserved `_deleted`) in the document indicates the document is deleted. Typically, these soft-delete documents have all normal data fields removed, and instead record customer deletion metadata, such as delete timestamp and reason.
    - Alternatively, use a separate database for "deleted" documents. Again, remove normal data fields and instead just include your own deletion metadata like deletion timestamp.
1. Run a migration process to retrieve all deleted documents, extract your data from the tombstone record and create a new, soft-deleted document to store the data.
1. Update all indexes to exclude soft-deleted documents.
    - Views and Search indexes can use guards in JavaScript indexing functions.
    - {{site.data.keyword.cloudant_short_notm}} Query indexes can use partial indexes.

## Advanced concerns
{: #advanced-concerns}
{: faq}

### If a document is deleted and later re-created with the same ID, what happens?
{: #deleted-and-recreated-document}
{: faq}

Creating a new version of a deleted document is internally represented as an update to the old document. So, in this case, the 90 day timer is reset; the tombstone metadata for the "old" version of the document are kept.

### Will the tombstone removal process affect database performance?
{: #impact-on-database-performance}
{: faq}

{{site.data.keyword.cloudant_short_notm}} has carried out extensive testing to validate that database performance is not affected by cleanup processing. Cleanup is a continuous process that does not result in spikes of activity.

### Will tombstone removal affect data storage bills?
{: #impact-on-storage-bills}
{: faq}

Customers may see small reductions in storage bills. The data used by deletion metadata is only a few bytes, so unless a database contains many millions of deleted documents, the reclaimed space will be very small.

### How can customers monitor tombstone removal?
{: #monitoring-tombstone-removal}
{: faq}

The `doc_del_count` field returned when [retrieving information about a database](/docs/apis/cloudant/cloudant-gen2#getdatabaseinformation){: external} will decrease as the cleanup process removes the deleted document metadata.

### Does this affect indexes (views, search, {{site.data.keyword.cloudant_short_notm}} Query)?
{: #impact-on-indexes}
{: faq}

The behaviour of indexes will not change.

Fully deleted documents do not appear in indexes, and the removal process ensures all indexes are up to date before cleaning up deletion metadata for each document.

### Are design documents and other system documents treated the same?
{: #design-and-system-documents}
{: faq}

Certain data resides in special documents in {{site.data.keyword.cloudant_short_notm}}:

- `_design` documents are subject to the removal process if fully deleted.
- `_local` documents are not subject to the removal process.

The following can appear document-like, but are in fact not documents so are not affected by the removal process:

- `_security` objects/properties.

### Will APIs expose cleanup status or removal timing?
{: #apis-for-cleanup-status}
{: faq}

{{site.data.keyword.cloudant_short_notm}} currently does not plan APIs for customers to view this information.
