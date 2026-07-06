---

copyright:
  years: 2020, 2022, 2026
lastupdated: "2026-06-08"

keywords: capacity settings, capacity limit, exceed limit, usage data, provisioned throughput capacity

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Pricing FAQ
{: #faq-pricing}
{: faq}
{: support}

{{site.data.keyword.cloudant_short_notm}} pricing is based on the provisioned throughput capacity that you set for your instance and the amount of data storage you use.
{: shortdesc}

With {{site.data.keyword.cloudantfull}}, you can increase or decrease your provisioned throughput capacity as needed and pay pro-rated hourly. The provisioned throughput capacity is a reserved number of reads per second, writes per second, and global queries per second allocated to an instance. The throughput capacity setting is the maximum usage level for a given second.

For more information, see [{{site.data.keyword.cloudant_short_notm}} Pricing](/docs/services/Cloudant?topic=Cloudant-usage-and-charges).

## Can I change my capacity setting?
{: #change-capacity}
{: faq}

You can change your provisioned throughput capacity and see your current capacity settings in the {{site.data.keyword.cloud_notm}} Dashboard. Locate your {{site.data.keyword.cloudant_short_notm}} instance resources list and choose **Manage** > **Capacity**
and click the **Upgrade Capacity** link. Choose a plan size between 1 and 100 units, where each unit provisions 100 reads per second, 50 writes per second, and 5 global queries per second.

## How do I know that I exceeded the capacity limit that I set?
{: #exceed-capacity}
{: faq}



The first 20 GB of storage comes free with the Standard plan. You can store as much data as you want, with any storage over the 20 GB limit charged per GB per hour.

## Where can I see my usage data?
{: #see-usage-data}
{: faq}

You can see your current and historical usage bills in the {{site.data.keyword.cloud_notm}} Dashboard. Go to **Manage** > **Billing and usage** > **Usage**. Here you can see the total charges and usage for the month by service, plan, or instance. Only the hourly costs that are accrued for the current month and time are available. At the end of the month, you can see the average provisioned throughput capacity for each field: `LOOKUPS_PER_MONTH`, `WRITES_PER_MONTH`, and `QUERIES_PER_MONTH`.
