---

copyright:
  years: 2019, 2026
lastupdated: "2026-06-04"

keywords: feature comparison, function comparison

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Comparing Apache CouchDB and {{site.data.keyword.cloudant_short_notm}}
{: #couchdb-and-cloudant}

The differences between the fully managed cloud service {{site.data.keyword.cloudantfull}} and self-managed open source Apache CouchDB still exist and are discussed here.
{: shortdesc}

The foundation of the {{site.data.keyword.cloudant_short_notm}} managed database service is the Apache CouchDB database. {{site.data.keyword.IBM_notm}} is active in the Apache CouchDB committee, employs members of the PMC, and commits most of its feature, functions, and enhancements back to the open source project. Over the last few years, {{site.data.keyword.IBM_notm}} made significant effort to align the core feature set of {{site.data.keyword.cloudant_short_notm}} and CouchDB. The {{site.data.keyword.cloudant_short_notm}} team contributed key features like {{site.data.keyword.cloudant_short_notm}} Query and Mango query language, full-text search, and partition queries to CouchDB. 

Apache CouchDB and {{site.data.keyword.cloudant_short_notm}} are nearly fully API compatible, which means they can serve as drop-in replacements for each other in your application.    

For more information, see the [API comparison guide](/docs/cloudant-gen2?topic=cloudant-gen2-comparison-of-ibm-cloudant-and-couchdb-api-endpoints) for a detailed breakdown of the API endpoints.

The following table shows the feature and function differences that you must be cognizant of when you use the Apache CouchDB and {{site.data.keyword.cloudant_short_notm}} data layer ecosystem. 

| Feature | CouchDB 3.x | {{site.data.keyword.cloudant_short_notm}} |
|---------|-------------|------------------------------------------ |
| Clustering | Yes | Yes |
| Fauxton Dashboard UI | Yes | Yes |
| MapReduce view | Yes | Yes |
| Mango and {{site.data.keyword.cloudant_short_notm}} Query | Yes | Yes |
| Full-text search | Yes, requires separate installer or container. | Yes |
| Partition queries | Yes | Yes |
| Shard splitting | Yes | No |
| Selector on `changes feed` | Yes | Yes |
| Rate limits | No | User-defined [provisioned throughput capacity](/docs/cloudant-gen2?topic=cloudant-gen2-usage-and-charges#provisioned-throughput-capacity-units) settings |
| Request size | 4 GB (default) | 10 MB |
| Attachment size | 4 GB (default) | 10 MB |
| Security auth | [CouchDB Auth](https://docs.couchdb.org/en/stable/intro/security.html#){: external} |  [{{site.data.keyword.cloud_notm}} IAM](/docs/cloudant-gen2?topic=cloudant-gen2-managing-access-for-cloudant) |
| LDAP | No | No |
{: caption="Feature and function differences between {{site.data.keyword.cloudant_short_notm}} and Apache CouchDB" caption-side="top"}
