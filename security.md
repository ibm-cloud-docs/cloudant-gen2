---

copyright:
  years: 2017, 2026
lastupdated: "2026-07-07"

keywords: dbaas data protection, tier 1 physical platforms, secure access control, data loss, corruption, byok, encryption, protection

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Security
{: #security}

Protecting application data for large-scale web and mobile apps can be complex,
especially with distributed and NoSQL databases.

Just as it reduces the effort of maintaining your databases
to keep them running and growing nonstop,
{{site.data.keyword.cloudantfull}} also ensures your data stays secure and protected.
{: shortdesc}

## Tier one physical platforms
{: #top-tier-physical-platforms}

The {{site.data.keyword.cloudant_short_notm}} DBaaS is
physically hosted on Tier-1 cloud infrastructure providers such as
{{site.data.keyword.cloud}}.
Therefore,
your data is protected by the network and physical security measures that are employed by those providers,
including (but not limited to):

   - Certifications - Compliance with SSAE16, SOC2 Type 1, ISAE 3402, ISO 27001, CSA, and other standards.
   - Access and identity management.
   - General physical security of data centers and network operations center monitoring.
   - Server hardening.
   - {{site.data.keyword.cloudant_short_notm}} gives you the flexibility to choose or switch among the different providers as your SLA and cost requirements change.



## Secure access control
{: #secure-access-control}

{{site.data.keyword.cloudant_short_notm}} has a multitude of built-in security features for you to control access to data:

| Feature | Description |
|--------|------------|
| Authentication | {{site.data.keyword.cloudant_short_notm}} is accessed by using an HTTPS API. Where the API endpoint requires it, the user is authenticated for every HTTPS request {{site.data.keyword.cloudant_short_notm}} receives.  |
| At-rest encryption | All data that is stored in an {{site.data.keyword.cloudant_short_notm}} instance is encrypted at rest. {{site.data.keyword.cloudant_short_notm}} manages the encryption keys for all environments.  **Comment from Glynn** Key Protect is not relevant at GA for Cloudant Gen2. {: note} |
| In-flight encryption | All access to {{site.data.keyword.cloudant_short_notm}} is encrypted by using HTTPS. |
| Client-side encryption | Customers can use client-side encryption to ensure that the data protection is controlled by the data owner and the data is never visible to the service provider. |
| TLS | {{site.data.keyword.cloudant_short_notm}} requires the use of TLS 1.2+. {{site.data.keyword.cloudant_short_notm}} strongly recommends that you do not pin certificates in your application. Certificates renew regularly, at least annually, and intermediate and root certificates could change when they do. {{site.data.keyword.cloudant_short_notm}} does not send out notifications before certificate renewals. We recommend that you keep your certificate truststore up to date with the latest root certificates. {{site.data.keyword.cloudant_short_notm}} acquires its certificates from Let's Encrypt. You can find their root certificates on the [Let's Encrypt Chains Of Trust](https://letsencrypt.org/certificates/){: external} page. {{site.data.keyword.cloudant_short_notm}} sends a notification if we move to a different certificate authority. |
| Public Endpoints | All {{site.data.keyword.cloudant_short_notm}} instances are provided with external endpoints that are publicly accessible. |
| Private Endpoints | Using private endpoints allows customers to connect to an {{site.data.keyword.cloudant_short_notm}} instance through the internal {{site.data.keyword.cloud}} network to avoid upstream application traffic from going over the public network and incurring bandwidth charges. For more information, see [Service Endpoint documentation](/docs/account?topic=account-service-endpoints-overview){: external}, and also. |
| CORS | Enable CORS support for specific domains by using the {{site.data.keyword.cloudant_short_notm}} Dashboard or API. For more information, see the [CORS API documentation](/docs/apis/cloudant/cloudant-gen2#getcorsinformation){: external}. |
{: caption="{{site.data.keyword.cloudant_short_notm}} security features" caption-side="top"}




## Protection against data loss or corruption
{: #protection-against-data-loss-or-corruption}

{{site.data.keyword.cloudant_short_notm}} has a number of features
to help you maintain data quality and availability:

| Feature | Description |
|--------|------------|
| Redundant and durable data storage | By default, {{site.data.keyword.cloudant_short_notm}} saves to disk three copies of every document to three different availability zones in a cluster. Saving the copies ensures that a working copy of your data is always available, regardless of failures. |
| Data Replication and export | You can replicate your databases continuously between clusters in different regions. Another option is to export data from {{site.data.keyword.cloudant_short_notm}} (in JSON format) to other locations or sources (such as your own data center) for added data redundancy. |
{: caption="{{site.data.keyword.cloudant_short_notm}} data quality and availability features" caption-side="top"}
