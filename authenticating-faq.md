---
copyright:
  years: 2020
lastupdated: "2026-08-24"

keywords: legacy, iam access controls, use only iam mode, generate service credentials, iam mode

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Authenticating with {{site.data.keyword.cloudant_short_notm}} FAQ
{: #faq-authenticating-cloudant}
{: faq}
{: support}

{{site.data.keyword.cloud}} Identity and Access Management (IAM) combines managing user identities, services, and access control into one approach. {{site.data.keyword.cloudantfull}} integrates with {{site.data.keyword.cloud_notm}} Identity and Access Management.
{: shortdesc}



## Advantages of IAM
{: #advantages-of-iam}
{: faq}

IAM has the following benefits:

- Managing access to {{site.data.keyword.cloudant_short_notm}} with the standard tooling of {{site.data.keyword.cloud_notm}}.
- Using credentials that you can easily revoke and rotate when you use {{site.data.keyword.cloud_notm}} IAM.

For more information about the advantages and disadvantages between these modes, see [Advantages and disadvantages of the two access control mechanisms](/docs/cloudant-gen2?topic=cloudant-gen2-managing-access-for-cloudant#advantages-and-disadvantages-of-the-two-access-control-mechanisms-ai).

## How can I create an instance by using the command line?
{: #create-iam-command-line}
{: faq}

See [Creating a service instance](/docs/cloudant-gen2?topic=cloudant-gen2-getting-started-with-cloudant#creating-an-ibm-cloudant-instance-on-ibm-cloud) for instructions on how to create a service instance by using the command line.

## How can I generate service credentials?
{: #find-service-credentials-iam}
{: faq}

See [Locating your credentials](/docs/cloudant-gen2?topic=cloudant-gen2-locating-your-service-credentials) for instructions on how to find your service credentials.

## How do I rotate my credentials?
{: #rotate-credentials}

In most cases, rotating credentials is a straight-forward process:

1. Generate a replacement service credential. For more information, see [How can I generate service credentials?](#find-service-credentials-iam).

1. Replace the current credential with the newly generated credential.

1. Delete the no-longer-used service credential.


The process is described in the following steps:

1. Generate a replacement service credential. For more information, see [How can I generate service credentials?](#find-service-credentials-iam).

1. Create a replication with the same settings but new credentials.

1. Monitor the new replication by using [Active Tasks](/docs/cloudant-gen2?topic=cloudant-gen2-active-tasks), or you can use [`_scheduler/jobs`](/docs/apis/cloudant/cloudant-gen2#getschedulerjobs).

1. When the `changes_pending` field for the new replication is a suitably low value for your requirements, the replication that uses the previous credentials can be deleted.

1. Delete the no-longer-used service credential.

Replications that use IAM API keys can be updated to use a new API key directly, without delaying the changes that are replicating.
{: important}
