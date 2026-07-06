---

copyright:
  years: 2020, 2023
lastupdated: "2023-04-04"

keywords: endpoints, service credentials, authentication,cloudant dashboard, curl, client libraries, IP allowlisting

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Securing your connection
{: #securing-your-connection-to-cloudant}

{{site.data.keyword.cloudantfull}} is accessed through an HTTP API where the traffic is encrypted in flight and at rest. This document describes the different parts that you use to connect to {{site.data.keyword.cloudant_short_notm}}:

- Endpoints - a public endpoint and a Virutal Private Endpoint (VPE)
- Service credentials
- Authentication
- Accessing the {{site.data.keyword.cloudant_short_notm}} Dashboard
- Programmatically accessing {{site.data.keyword.cloudant_short_notm}} through client SDKs
{: shortdesc}

## Endpoints
{: #endpoints-sc}

{{site.data.keyword.cloudant_short_notm}} is accessed through HTTP API endpoints. The endpoints for an instance are
shown in both the URL field of the Service Credentials that are generated for the instance, and in **Account** > **Settings** of the
{{site.data.keyword.cloudant_short_notm}} Dashboard.

Therefore, all {{site.data.keyword.cloudant_short_notm}} HTTP endpoints must be accessed over TLS and prefaced by `https://`.

### Public endpoints
{: #public-endpoints-sc}

The publicly facing external endpoint is shown in the following example:

`https://00000000-0000-0000-0000-00000000.abc.cloudant.eu-de.dataservices.appdomain.cloud`


### Virtual Private Endpoint
{: #virtual-private-endpoints-sc}

The {{site.data.keyword.cloud_notm}} VPE URL is shown in the following example:

`https://00000000-0000-0000-0000-00000000.private.abc.cloudant.eu-de.dataservices.appdomain.cloud`

For more information on accessing your database on the private network, see the [Service Endpoints](/docs/cloudant-gen2?topic=cloudant-gen2-connecting) documentation.



## Service credentials
{: #service-credentials-sc}

To generate service credentials for {{site.data.keyword.cloudant_short_notm}} by using the {{site.data.keyword.cloud_notm}}
Dashboard, see [Creating an {{site.data.keyword.cloudant_short_notm}} instance on {{site.data.keyword.cloud_notm}}](/docs/cloudant-gen2?topic=cloudant-gen2-getting-started-with-cloudant). To generate service credentials from
the {{site.data.keyword.cloud_notm}} CLI, see [Creating credentials for your {{site.data.keyword.cloudant_short_notm}}
service](/docs/cloudant-gen2?topic=cloudant-gen2-creating-an-ibm-cloudant-instance-on-ibm-cloud-by-using-the-ibm-cloud-cli#creating-an-ibm-cloudant-instance-on-ibm-cloud-by-using-the-ibm-cloud-cli).

The following example shows service credentials for an {{site.data.keyword.cloudant_short_notm}} instance:

```json
{
  "apikey": "...",
  "iam_apikey_description": "Auto-generated for key crn:v1:bluemix:public:cloudant-cdp-dev:eu-de:a/...:...:resource-key:...",
  "iam_apikey_id": "ApiKey-82b20656-9c5e-4c5c-9418-45962fc96c92",
  "iam_apikey_name": "test-key",
  "iam_role_crn": "...",
  "iam_serviceid_crn": "...",
  "url": "https://00000000-0000-0000-0000-00000000.abc.cloudant.eu-de.dataservices.appdomain.cloud",
  "vpe_url": "https://00000000-0000-0000-0000-00000000.private.abc.cloudant.eu-de.dataservices.appdomain.cloud"
}
```
{: codeblock}

The service credentials include the following fields:

| Field | Purpose |
|------|--------|
| `apikey` | The IAM API key. |
| `iam_apikey_description` | Description of the IAM API key. |
| `iam_apikey_id` | Unique ID of the IAM API key. |
| `iam_apikey_name` | ID of the IAM API key. |
| `iam_role_crn` | The IAM role that the IAM API key has. |
| `iam_serviceid_crn`	| The CRN of the service ID. |
| `url`	| The public URL of the {{site.data.keyword.cloudant_short_notm}} instance.|
| `vpe_url`	| The VPE URL of the {{site.data.keyword.cloudant_short_notm}} instance, where traffic flows on the private network. |
{: caption="Service credential fields" caption-side="top"}







## Authentication
{: #authentication-overview-sc}

Authentication uses the IBM IAM service. See [Locating your Service Credentials](/docs/cloudant-gen2?topic=cloudant-gen2-locating-your-service-credentials) for more information.

In use, the IAM API key is exchanged for time-limited access token which is included in API requests to {{site.data.keyword.cloudant_short_notm}}. The Cloudant SDKs handle the API key to token exchange and token refresh automatically.



## {{site.data.keyword.cloudant_short_notm}} Dashboard
{: #ibm-cloudant-dashboard-sc}

You can open the {{site.data.keyword.cloudant_short_notm}} Dashboard for your instance by going to the Manage tab of
the {{site.data.keyword.cloud_notm}} Dashboard instance details page. You can use either `Launch` to open the dashboard in a new browser tab. You can do the following tasks by using the {{site.data.keyword.cloudant_short_notm}} Dashboard:

- Perform create, read, update, and delete on {{site.data.keyword.cloudant_short_notm}} databases, documents, and indexes.
- Set up and view replication jobs.
- View active tasks.

## Programmatic access with client SDKs
{: #programmatic-access-client-sdks-sc}

{{site.data.keyword.cloudant_short_notm}} has official client libraries for Java&trade;, Node.js, Go and Python. For more information, see the [client libraries documentation](/docs/cloudant-gen2?topic=cloudant-gen2-client-libraries#client-libraries) to access the libraries, and see examples for connecting to an {{site.data.keyword.cloudant_short_notm}}
instance from each.
