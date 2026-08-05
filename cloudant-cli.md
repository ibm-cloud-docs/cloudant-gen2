---

copyright:
  years: 2026
lastupdated: "2026-08-05"

subcollection: cloudant-gen2

keywords: Cloudant CLI, Cloudant command line, Cloudant terminal, Cloudant shell, Gen 2 CLI

content-type: cli-docs
---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.cloudant_short_notm}} CLI
{: #cloudant-cli}

[Gen 2]{: tag-purple}

To interact with {{site.data.keyword.cloudant_short_notm}} on Gen 2 through the CLI you must use the {{site.data.keyword.cloud_notm}} `resource` CLI commands. For more information, see [General {{site.data.keyword.cloud_notm}} CLI (ibmcloud) commands](/docs/cli?topic=cli-ibmcloud_cli).
{: shortdesc}


The {{site.data.keyword.cloudant_short_notm}} plug-in supports only Gen 1 instances. For Gen 2 instances, use the `resource` CLI commands.
{: .note}




## Prerequisites
{: #cloudant-cli-prereq}

* Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started).
* Install `jq` for JSON parsing. You can install it using your system's package manager (for example, `brew install jq` on macOS, `apt-get install jq` on Ubuntu).
* Use [`ibmcloud login` command](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login) for logging in to your {{site.data.keyword.cloud_notm}} account.

You're notified on the command line when updates to the {{site.data.keyword.cloud_notm}} CLI are available. Be sure to keep your CLI up to date so that you can use the latest commands.
{: tip}


## Understanding RESOURCE_ID
{: #cloudant-cli-resource-id}


In the commands below, `RESOURCE_ID` refers to the Cloud Resource Name (CRN) or GUID of your {{site.data.keyword.cloudant_short_notm}} service instance. You can find this by running `ibmcloud resource service-instances` or in the {{site.data.keyword.cloud_notm}} console.

To get the GUID or CRN of your service instance:

```sh
# Get GUID of the service instance by service name
ibmcloud resource service-instance SERVICE_NAME --guid
# Get CRN ID of the service instance by service name
ibmcloud resource service-instance SERVICE_NAME --crn
```
{: pre}

## CLI command reference
{: #cloudant-commands}

The following sections show CLI commands for common {{site.data.keyword.cloudant_short_notm}} operations.

### Retrieve service URL
{: #cloudant-cli-url}

Retrieve the service URL from the given {{site.data.keyword.cloudant_short_notm}} resource.

```sh
# Public URL
ibmcloud resource service-instance RESOURCE_ID -o json | jq '.[0].extensions.dataservices.connection.public_endpoint_url'
# VPE (private) URL
ibmcloud resource service-instance RESOURCE_ID -o json | jq '.[0].extensions.dataservices.connection.vpe_url'
```
{: pre}

### View capacity
{: #cloudant-cli-capacity}

View the provisioned throughput capacity of an {{site.data.keyword.cloudant_short_notm}} instance.

```sh
ibmcloud resource service-instance RESOURCE_ID -o json | jq '.[0].extensions.dataservices.cloudant.capacity_units'
```
{: pre}

### Update capacity
{: #cloudant-cli-capacity-update}

Update the provisioned throughput capacity of an {{site.data.keyword.cloudant_short_notm}} instance.

```sh
ibmcloud resource service-instance-update RESOURCE_ID -p '{"dataservices": {"cloudant": {"capacity_units" : BLOCKS}}}'
```
{: pre}

### View audit events
{: #cloudant-cli-audit-events}

View event types configured for {{site.data.keyword.atracker_full_notm}} on the {{site.data.keyword.cloudant_short_notm}} instance. Note that management events are always enabled and cannot be disabled.

```sh
ibmcloud resource service-instance RESOURCE_ID -o json | jq '.[0].extensions.dataservices.cloudant.configuration.audit'
```
{: pre}

### Update audit events
{: #cloudant-cli-audit-events-update}

Update event types configured for {{site.data.keyword.atracker_full_notm}} on the {{site.data.keyword.cloudant_short_notm}} instance. Note that management events are always enabled and cannot be disabled.

```sh
ibmcloud resource service-instance-update RESOURCE_ID -p '{"dataservices": {"cloudant": {"configuration" : {"audit" : {"data_events": true}}}}}'
```
{: pre}

### View current throughput
{: #cloudant-cli-throughput}

View current provisioned throughput capacity consumption for the {{site.data.keyword.cloudant_short_notm}} instance, including reads, writes, and global queries per second.

Use the {{site.data.keyword.cloud_notm}} Monitoring API to [extract and view metrics](/docs/monitoring?topic=monitoring-metrics_api).
For CLI usage `curl` is a good option, though there are many available options.

This example obtains the rate over the range one minute before the most recent available data point.

Replace the example Bearer token below with your own valid {{site.data.keyword.cloud_notm}} IAM token.
{: .note}

```sh
curl https://us-south.monitoring.cloud.ibm.com/api/prometheus/api/v1/query?query=rate(ibm_cloudant_permitted_operations_total%5B1m%5D) \
  -H 'Authorization: Bearer A1b2C3QiOiIyMDE4MDgxNDAwMDAwMDAwMDAwMDBjNzYwNzY2YjYxYjYwYjYwIiwidHlwIjoiSldUIiwiYWxnIjoiUlMyNTYifQ.eyJzdWIiOiJ1c2VyQGdtYWlsLmNvbSIsImF1ZCI6Imh0dHBzOi8vaWF1LmNsb3VkLmlibS5jb20iLCJpYXQiOjE2ODg4ODg4ODgsImV4cCI6MTY4ODg5MjQ4OCwiaXNzIjoiaHR0cHM6Ly9pYXUuY2xvdWQuaWJtLmNvbSIsInNjb3BlIjpbImNsb3VkLnJlYWRlciJdfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c' \
  -H 'IBMInstanceID: RESOURCE_ID'
```
{: pre}
