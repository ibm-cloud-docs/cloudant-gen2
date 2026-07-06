---

copyright:
  years: 2020, 2022, 2026
lastupdated: "2026-06-09"

keywords: isolation for IBM Cloudant, service endpoints for IBM Cloudant, private network for IBM Cloudant, network isolation in IBM Cloudant, non-public routes for IBM Cloudant, private connection for IBM Cloudant, private connectivity for IBM Cloudant

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to {{site.data.keyword.cloudant_short_notm}} on the private network
{: #connecting-on-the-private-network}

To connect to {{site.data.keyword.cloudant_short_notm}}, a Virtual Private Endpoint (VPE) for VPC must be created to establish a secure private connection. Credentials must then be created to authenticate with IAM.
{: shortdesc}

## Locating your {{site.data.keyword.cloudant_short_notm}} VPE URL
{: #locating-your-vpe-url}

All {{site.data.keyword.cloudant_short_notm}} instances have two urls:

- `url` - the public URL of your {{site.data.keyword.cloudant_short_notm}} instance.
- `vpe_url` - the VPE URL for your {{site.data.keyword.cloudant_short_notm}} instance.

The `vpe_url` will have the word `private` in the URL, for example:  `https://00000000-0000-0000-0000-00000000.private.abc.cloudant.eu-de.dataservices.appdomain.cloud` and is only accessible from within your database's VPC.

## Virtual Private Endpoint (VPE) gateways
{: #virtual-private-endpoint-gateways}

Application virtual machines running in your VPC can connect to your {{site.data.keyword.cloudant_short_notm}} instance by using a Virtual Private Endpoint (VPE) gateway. When connected, network traffic between your application and the database is routed on the private network. This approach avoids data egress charges by avoiding routing traffic on the public network.

As a VPE Gateway is an extension of your existing VPC network, you can use the same security groups and network ACLs to control access to the database from your network.

Read more about [setting up a VPE](/docs/vpc?topic=vpc-about-vpe).

## Using the VPE URL for {{site.data.keyword.cloudant_short_notm}}
{: #using-vpe-url-for-cloudant}

After connectivity is established between your application's VPC and your {{site.data.keyword.cloudant_short_notm}} instance, you can use the VPE URL to access your {{site.data.keyword.cloudant_short_notm}} instance in your application code.

Simply use the `vpe_url` instead of the (public) `url` and the traffic between application VMs and the database will flow on the private network.
