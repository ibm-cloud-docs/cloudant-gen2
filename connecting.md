---

copyright:
  years: 2019, 2023, 2026
lastupdated: "2026-06-09"

keywords: endpoints, service credentials, authentication, ibm cloudant dashboard, curl, client libraries, IP allowlisting

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Connecting
{: #connecting}

{{site.data.keyword.cloudantfull}} is accessed through an HTTP API. You can see the different parts that you use to connect to {{site.data.keyword.cloudant_short_notm}} in the following list:
- Endpoints
- Service keys
- Authentication
- Accessing the {{site.data.keyword.cloudant_short_notm}} Dashboard
- Programmatically accessing {{site.data.keyword.cloudant_short_notm}} by using [curl](https://curl.haxx.se/){: external} or client libraries
{: shortdesc}

## Endpoints
{: #endpoints}

{{site.data.keyword.cloudant_short_notm}} is accessed through HTTP API endpoints. The endpoints for an instance are
shown in both the URL field of the Service Credentials that are generated for the instance, and in **Account** > **Settings** of the
{{site.data.keyword.cloudant_short_notm}} Dashboard.

All {{site.data.keyword.cloudant_short_notm}} HTTP endpoints must be accessed over TLS and prefaced by `https://`.

The publicly facing external endpoint is shown in the following example:

`00000000-0000-0000-0000-00000000.abc.cloudant.eu-de.dataservices.appdomain.cloud`

- `00000000-0000-0000-0000-00000000` is the unique identifier for the instance.
- `abc` is the ingress domain for this instance in which the instance is deployed. It will be a combination of three letters.
- `cloudant` is the service name.
- `eu-de` is the data center in which the instance is deployed.
- `dataservices.appdomain.cloud` is the domain for the IBM's data services.

Private (internal) endpoints are availble on all instances. An example {{site.data.keyword.cloud_notm}} Private (internal) network endpoint is shown in the following example:

`00000000-0000-0000-0000-00000000.private.abc.cloudant.eu-de.dataservices.appdomain.cloud`

Note the word `private` in the URL.
{: note}


## Service credentials
{: #service-credentials-con}

To generate service credentials for {{site.data.keyword.cloudant_short_notm}} by using the {{site.data.keyword.cloud_notm}}
Dashboard, see the [Creating an {{site.data.keyword.cloudant_short_notm}} instance on {{site.data.keyword.cloud_notm}}](/docs/cloudant-gen2?topic=cloudant-gen2-getting-started-with-cloudant) tutorial. To generate service credentials from
the {{site.data.keyword.cloud_notm}} CLI, see [Creating an instance with CLI](/docs/cloudant-gen2?topic=cloudant-gen2-creating-an-ibm-cloudant-instance-on-ibm-cloud-by-using-the-ibm-cloud-cli#creating-an-ibm-cloudant-instance-on-ibm-cloud-by-using-the-ibm-cloud-cli).

The following example shows service credentials for an {{site.data.keyword.cloudant_short_notm}} instance:

```json
{
    "guid": "e46688a4-7155-4ee3-92ed-d64baa0f6e17",
    "id": "crn:v1:bluemix:public:cloudant-cdp-dev:us-east:a/92a3346cfc4a:00000000-0000-0000-0000-00000000:resource-key:e46688a4-7155-4ee3-92ed-d64baa0f6e17",
    "url": "/v2/resource_keys/e46688a4-7155-4ee3-92ed-d64baa0f6e17",
    "created_at": "2026-06-01T13:10:13.722032774Z",
    "updated_at": "2026-06-01T13:10:13.722032774Z",
    "deleted_at": null,
    "name": "test-key",
    "account_id": "92a3346cfc4a",
    "resource_group_id": "...",
    "source_crn": "crn:v1:bluemix:public:cloudant-cdp-dev:us-east:a/92a3346cfc4a:00000000-0000-0000-0000-00000000::",
    "state": "active",
    "credentials": {
        "apikey": "...",
        "iam_apikey_description": "Auto-generated for key crn:v1:bluemix:public:cloudant-cdp-dev:us-east:a/REDACTED:00000000-0000-0000-0000-00000000:resource-key:e46688a4-7155-4ee3-92ed-d64baa0f6e17",
        "iam_apikey_id": "ApiKey-82b20656-9c5e-4c5c-9418-45962fc96c92",
        "iam_apikey_name": "test-key",
        "iam_role_crn": "...",
        "iam_serviceid_crn": "...",
        "url": "https://00000000-0000-0000-0000-00000000.abc.cloudant.eu-de.dataservices.appdomain.cloud",
        "vpe_url": "https://00000000-0000-0000-0000-00000000.private.abc.cloudant.eu-de.dataservices.appdomain.cloud"
    },
    "iam_compatible": true,
    "onetime_credentials": false,
    "resource_instance_url": "/v2/resource_instances/00000000-0000-0000-0000-00000000",
    "crn": "crn:v1:bluemix:public:cloudant-cdp-dev:us-east:a/92a3346cfc4a:00000000-0000-0000-0000-00000000:resource-key:e46688a4-7155-4ee3-92ed-d64baa0f6e17"
}
```
{: codeblock}

The service credentials include the following fields:

`apikey`
:  The IAM API key.

`iam_apikey_description`
:  Description of the IAM API key.

`iam_apikey_name`
:  ID of the IAM API key.

`iam_role_crn`
:  The IAM role that the IAM API key has.

`iam_serviceid_crn`
:  The CRN of the service ID.


## {{site.data.keyword.cloudant_short_notm}} Dashboard
{: #ibm-cloudant-dashboard}

You can open the {{site.data.keyword.cloudant_short_notm}} Dashboard for your instance by going to the Manage tab of
the {{site.data.keyword.cloud_notm}} Dashboard instance details page. You can use either `Launch` or `Launch Cloudant Dashboard` to open the dashboard in a new browser tab. You can do the following tasks by using the {{site.data.keyword.cloudant_short_notm}} Dashboard:

- Perform create, retrieve, update, delete on {{site.data.keyword.cloudant_short_notm}} databases, documents, and indexes.
- Set up and view replication jobs.
- View active tasks.

## Programmatic access
{: #programmatic-access}

### Client libraries
{: #client-libraries-overview}

{{site.data.keyword.cloudant_short_notm}} has official, supported client libraries for Java&trade;, Node.js, Python, Swift, Go, and Mobile. For more information, see the [client libraries documentation](/docs/cloudant-gen2?topic=cloudant-gen2-client-libraries#client-libraries) to access the libraries, and see examples for connecting to an {{site.data.keyword.cloudant_short_notm}}
instance from each.
