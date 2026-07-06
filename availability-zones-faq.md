---
copyright:
  years: 2020
lastupdated: "2026-06-04"

keywords: availability zones, single-zone region, multi-zone region,  standard plan

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Availability zones FAQ
{: #faq-availability-zones}
{: faq}
{: support}


## What is an availability zone?
{: #what-availability-zone}
{: faq}

When you create an instance, after you select the {{site.data.keyword.cloudant_short_notm}} tile, you must select a region. Each region contains multiple availability zones. An availability zone is an {{site.data.keyword.cloud}} location that hosts your compute and storage resources. All {{site.data.keyword.cloudant_short_notm}} instances automatically deploy into a multi-zone region. Having multiple availability zones, each containing a copy of your {{site.data.keyword.cloudant_short_notm}} JSON data, secondary indexes, and attachments means a {{site.data.keyword.cloudant_short_notm}} service is able to survive the loss of an availability zone. 
