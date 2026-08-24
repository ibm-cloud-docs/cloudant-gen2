---

copyright:
  years: 2015, 2026
lastupdated: "2026-08-24"

keywords: capacity, provisioned throughput capacity, monitor usage, IBM Cloud Monitoring, metrics, denied requests, rate limiting, reads, writes, global queries

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring capacity usage
{: #monitoring-capacity-usage}

{{site.data.keyword.cloudant_short_notm}} exposes metrics through {{site.data.keyword.mon_full_notm}} that let you monitor how your instance is consuming its provisioned throughput capacity and identify when you are approaching or exceeding your limits.
{: shortdesc}

For information about how provisioned throughput capacity is allocated and how to view or change it, see [Scaling provisioned throughput capacity](/docs/cloudant-gen2?topic=cloudant-gen2-usage-and-charges#scaling-provisioned-throughput-capacity).

## Setting up monitoring
{: #capacity-setup-monitoring}

{{site.data.keyword.mon_full_notm}} platform metrics are the recommended way to monitor your {{site.data.keyword.cloudant_short_notm}} instance. Platform metrics provide real-time visibility into throughput consumption, denied requests, data usage, and more.

To set up monitoring, see [{{site.data.keyword.mon_full_notm}} integration](/docs/cloudant-gen2?topic=cloudant-gen2-monitor-ibm-cloud-pm). After setup, you can open the pre-built **{{site.data.keyword.cloudant_short_notm}}** dashboard directly from your instance's **Actions** menu in the {{site.data.keyword.cloud_notm}} Dashboard.

## Understanding the monitoring metrics
{: #capacity-metrics}

The usage metrics for {{site.data.keyword.cloudant_short_notm}} provide visibility into permitted operations broken down by reads, writes, and global queries, and the number of denied requests that indicate rate-limiting.

### Permitted operations
{: #capacity-metrics-permitted}

The `ibm_cloudant_permitted_operations_total` metric shows the total number of billable operations (reads, writes, and global queries) that have been successfully processed by the instance. In the {{site.data.keyword.mon_full_notm}} dashboard, you can segment this metric by the `ibm_cloudant_operation_type` attribute to see the breakdown across each request class.

Use this metric to understand how your application is consuming its provisioned throughput. Comparing actual consumption against the `ibm_cloudant_provisioned_throughput` metric helps you confirm whether your current capacity allocation is appropriate.

### Denied requests
{: #capacity-metrics-denied}

The `ibm_cloudant_denied_requests_total` metric shows the number of requests that were rejected because the instance exceeded its provisioned throughput capacity. Denied requests result in an HTTP [`429` Too Many Requests](/docs/apis/cloudant/cloudant-gen2#error-handling){: external} response.

You can segment this metric by the `ibm_cloudant_operation_type` attribute to see denials broken down across reads, writes, and global queries. Exceeding the limit for one request class does not block the others: for example, exceeding the global query allowance does not prevent reads and writes from succeeding.

Consider setting up alerts on `ibm_cloudant_denied_requests_total` to be notified when your application experiences sustained rate-limiting.
{: tip}

If you observe a consistently elevated denied request rate, consider [increasing your provisioned throughput capacity](/docs/cloudant-gen2?topic=cloudant-gen2-usage-and-charges#scaling-provisioned-throughput-capacity).

### HTTP requests
{: #capacity-metrics-requests}

The `ibm_cloudant_permitted_requests_total` metric counts all permitted HTTP requests made against the instance. There is not a one-to-one mapping between HTTP requests and billable operations: a single bulk read or bulk write request can consume multiple read or write operations. Use `ibm_cloudant_permitted_operations_total` to understand throughput consumption, and `ibm_cloudant_permitted_requests_total` for overall request volume.

### Data usage
{: #capacity-metrics-data}

The `ibm_cloudant_data_usage_bytes` metric shows the total amount of data stored in the instance, in bytes. For information about how data storage is measured and billed, see [Calculating data usage for the Standard plans](/docs/cloudant-gen2?topic=cloudant-gen2-usage-and-charges#data-usage).

The `ibm_cloudant_databases` metric shows the number of databases in the instance. For information about the database count limit per instance, see [Databases](/docs/cloudant-gen2?topic=cloudant-gen2-limits#databases-overview).

### Provisioned throughput
{: #capacity-metrics-provisioned}

The `ibm_cloudant_provisioned_throughput` metric reports the number of provisioned throughput capacity blocks currently allocated to the instance. For the read, write, and global query allowance per block, see [Scaling provisioned throughput capacity](/docs/cloudant-gen2?topic=cloudant-gen2-usage-and-charges#scaling-provisioned-throughput-capacity).

Use this metric alongside `ibm_cloudant_permitted_operations_total` and `ibm_cloudant_denied_requests_total` to understand how your actual usage compares to your allocated capacity.

## Metrics reference
{: #capacity-metrics-reference}

The following metrics are available for {{site.data.keyword.cloudant_short_notm}} instances. You can query all metrics programmatically using the [Monitoring API](/docs/monitoring?topic=monitoring-metrics_api).

| Metric name | Type | Description |
| ----------- | ---- | ----------- |
| `ibm_cloudant_provisioned_throughput` | Gauge | The number of provisioned throughput capacity blocks allocated to the instance. |
| `ibm_cloudant_permitted_requests_total` | Counter | The total number of permitted HTTP requests made against the instance. |
| `ibm_cloudant_permitted_operations_total` | Counter | The total number of permitted billable operations (reads, writes, and global queries) against the instance. Segment by `ibm_cloudant_operation_type` to see the per-class breakdown. |
| `ibm_cloudant_denied_requests_total` | Counter | The total number of requests denied because the provisioned throughput capacity was exceeded. Segment by `ibm_cloudant_operation_type` to see the per-class breakdown. |
| `ibm_cloudant_data_usage_bytes` | Gauge | The amount of data stored in the instance, in bytes. |
| `ibm_cloudant_databases` | Gauge | The number of databases in the instance. |
{: caption="{{site.data.keyword.cloudant_short_notm}} usage metrics" caption-side="bottom"}

### Querying metrics with the API
{: #capacity-metrics-api-query}

You can query metrics programmatically using the [Monitoring API](/docs/monitoring?topic=monitoring-metrics_api). The following example uses `curl` to query the rate of permitted operations over a one-minute window.

Replace the Bearer token with your [IAM OAuth token](/docs/monitoring?topic=monitoring-mon-curl#mon-curl-query) and set the `time` parameter to your evaluation timestamp.
{: note}

```sh
curl "https://us-south.monitoring.cloud.ibm.com/api/v2/promql/query?query=rate(ibm_cloudant_permitted_operations_total%7Bibm_service_instance%3D%22RESOURCE_ID%22%7D%5B1m%5D)&time=TIMESTAMP" \
  -H 'Authorization: Bearer IAM_TOKEN' \
  -H 'IBMInstanceID: MONITORING_RESOURCE_ID'
```
{: pre}
