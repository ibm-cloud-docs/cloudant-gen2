---

copyright:
  years: 2015, 2022, 2026
lastupdated: "2026-06-09"

keywords: cross-domain, security, configuration endpoints, json format, dashboard, set CORS configuration, read CORS configuration, IBM Cloudant Dashboard, same origin security policy

subcollection: Cloudant

---

{{site.data.keyword.attribute-definition-list}}

# How Cross-origin resource sharing (CORS) works
{: #cross-origin-resource-sharing}

[CORS](https://www.w3.org/wiki/CORS){: external} is a mechanism that allows resources
such as JSON documents in an {{site.data.keyword.cloudantfull}} database to be requested
from JavaScript running in a browser - specifically when host website and the  {{site.data.keyword.cloudant_short_notm}}
database are served on different domains.
{: shortdesc}

These "cross-domain" requests would normally be forbidden by web browsers. The requests use the [same origin security policy](https://en.wikipedia.org/wiki/Same-origin_policy){: external}.

CORS defines a way in which the browser and the server interact to determine whether or not to allow the request.
For {{site.data.keyword.cloudant_short_notm}},
CORS might be a good solution in the following use cases.

1.	You have a website on `https://www.example.com`, and you want scripts on this website that can access data from `https://00000000-0000-0000-0000-00000000.abc.cloudant.eu-de.dataservices.appdomain.cloud`.

	To make this access possible,
	add `https://www.example.com` to your list of allowed origins.
	The effect is that scripts that are loaded from this domain are then
	permitted to make Fetch (formerly known as "Ajax" or "XMLHttpRequest") requests to your {{site.data.keyword.cloudant_short_notm}} databases.
	By using HTTP authorization with CORS requests,
	users of your application can access only their database.

2.	You want to allow third parties access to your database.

	For example, if you have a database that includes product information, add their domain to your list of allowed origins. After that, you can give sales partners access to the information from JavaScript running on their domain.
	The effect is that scripts that run on their website can access your {{site.data.keyword.cloudant_short_notm}} database.

## Browser support
{: #browser-support}

CORS is supported by all current versions of commonly used browsers.


## Security
{: #security-overview}

Storing sensitive data in databases that can be accessed by using CORS is a potential security risk.
When you place a domain in the list of allowed origins,
you're trusting all the JavaScript from the domain.
If the web application that runs on the domain is running malicious code or has security vulnerabilities,
sensitive data in your database might be exposed.

To reduce the risk of man-in-the-middle attacks, follow these guidelines:

-	Don't allow CORS requests from all origins.
	In other words,
	do not set `"origins": ["*"]` unless you're certain that you want to meet the following conditions:
	-	You want to allow all data in your databases to be publicly accessible.
	-	User credentials that give permission to modify data are never used in a browser.
-	Ensure that web applications that run on allowed origin domains are trusted
	and do not have security vulnerabilities.


## Configuring CORS

CORS configuration is set at the service instance level.

### Setting CORS configuration when creating a Cloudant instance

Using the `ibmcloud` CLI, you can set the CORS configuration when creating a Cloudant instance:

```sh
ibmcloud resource service-instance-create my_cors_enabled_instance cloudantnosqldb standard-gen2 eu-de --parameters '{"dataservices": {"cloudant":{"capacity_units": 2, "configuration": {"cors": {"enabled": true, "origins": ["https://example.com"]}}}}}'
```
{: codeblock}

Unpacking the `parameters` JSON for readability:

```json
{
  "dataservices": {
    "cloudant": {
      "capacity_units": 2,
      "configuration": {
        "cors": {
          "enabled": true,
          "origins": [
            "https://example.com"
          ]
        }
      }
    }
  }
}
```
{: codeblock}

- `capacity_units` defines the provisioned throughput capacity of the {{site.data.keyword.cloudant_short_notm}} instance.
- `enabled` sets whether CORS is enabled or not.
- `origins` is the list of origin URLs that are allowed to access the {{site.data.keyword.cloudant_short_notm}} instance via CORS.

## Configuring CORS for an existing Cloudant instance

To update the CORS configuration for an existing {{site.data.keyword.cloudant_short_notm}} instance, use the `ibmcloud` CLI to update the instance parameters:

```sh
ibmcloud resource service-instance-update ac8c67ee-edfd-4cb2-8d76-44dbd5ed6bea --parameters '{"dataservices": {"cloudant":{"capacity_units": 2, "configuration": {"cors": {"enabled": true, "origins": ["https://example.com","https://another.com"]}}}}}'
```
{: codeblock}

where `ac8c67ee-edfd-4cb2-8d76-44dbd5ed6bea` is the ID of your {{site.data.keyword.cloudant_short_notm}} instance.

Service instance changes may take a minute or two to take effect.
{: note}
