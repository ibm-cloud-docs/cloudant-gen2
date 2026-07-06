---

copyright:
  years: 2026
lastupdated: "2026-06-25"

keywords: cloudant, gen 2, benefits

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Advantages of {{site.data.keyword.cloudant_short_notm}} Gen 2
{: #advantages-gen2}

## The platform
{: #platform}

{{site.data.keyword.cloudant_short_notm}} Gen 2 shares the same core platform as all of the other IBM Cloud Databases, based on highly secure software-defined networking architecture and ideal for cloud-native applications.

## High availability
{: #high-availability}

{{site.data.keyword.cloudant_short_notm}} distributes copies of data and indexes across a region's availability zones to ensure that your data is always available and that the database can continue to operate with the loss of a zone.



## IAM and security
{: #iam-and-security}

{{site.data.keyword.cloudant_short_notm}} is fully integrated with IBM Cloud Identity and Access Management (IAM) to allow the right users to access the right resources at the right time.

## Private networking
{: #private-networking}

{{site.data.keyword.cloudant_short_notm}} Gen 2 supports VPE URLs which are only available on the region's private network. Using the private network attracts no egress charges and makes for the most secure method of connecting an application and its database.

## Lucene 10
{: #lucene10}

{{site.data.keyword.cloudant_short_notm}} Gen 2 uses the new and improved search engine Lucene 10 to support free-text search and flexible queries. The Cloudant Search API remains the same, but the underlying search engine is now based on a newer version of Lucene.

## Auto-tombstone removal
{: #tombstone-removal}

Tombstones (the data that remains after a document is deleted) are automatically completely removed after 90 days in {{site.data.keyword.cloudant_short_notm}} Gen 2. This reduces database size and improves the performance of the changes feed, replication, and index building.

## QuickJS
{: #quickjs}

The QuickJS engine is now used for all JavaScript processing, which provides faster performance with a smaller memory footprint than its predecessor. Read more in the [Cloudant Blog](https://blog.cloudant.com/2024/10/29/QuickJS-for-Faster-Index-Builds.html){: external}.

## New Reduces
{: #new-reduces}

New MapReduce reducers provide easy access to the highest and lowest values from an index, or the first and last values from key groups. Read more in the [Cloudant Blog](https://blog.cloudant.com/2025/05/23/New-Reducers.html){: external}.
