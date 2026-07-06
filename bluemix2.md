---

copyright:
  years: 2015, 2026
lastupdated: "2026-06-23"

keywords: standard plan, request class, provisioned throughput capacity, consumption, capacity, monitor usage, data usage, size limits, locations, tenancy, authentication methods, high availability, disaster recovery, backup, support

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Plans and provisioning
{: #plans-and-provisioning}

{{site.data.keyword.cloudantfull}}'s Standard plan allows you to scale your
application to meet your needs. API capacity can be scaled up and down to meet your application's needs, and disk usage is billed based on the amount of data stored.

## Standard plan
{: #standard-plan}

The available plan for {{site.data.keyword.cloudantfull}} is:

- **Standard Plan** – API usage for read, write and query operations is billed on the provisioned capacity configured for your {{site.data.keyword.cloudant_short_notm}} instance. Storage is billed on the amount of data used to store primary JSON data and any secondary indexes, with the first 20GB free.

Refer to the {{site.data.keyword.cloud_notm}} Cost Estimator in the dashboard for charges at different capacities and currencies, and the [Usage and charges](/docs/cloudant-gen2?topic=cloudant-gen2-usage-and-charges){: external} information for examples to estimate costs.

### Standard plan summary

| Feature                          | **Standard Plan**                                      |
|----------------------------------|--------------------------------------------------------|
| **Cost**                         | Pay-as-you-go or subscription                          |
| **Use-case**                     | Development and production workloads                                   |
| **Throughput capacity**          | Starts at 100 reads/sec, 50 writes/sec, 5 global queries/sec; scalable in [provisioned throughput capacity units](/docs/cloudant-gen2?topic=cloudant-gen2-usage-and-charges#provisioned-throughput-capacity-units)       |
| **Throughput scaling**                  | Throughput scalable via UI or API                           |
| **Included storage**                | 20GB |
| **Extra storage**                | Charged per extra GB                   |
| **Instance limit**              | Unlimited                                               |
| **Included features**            | All {{site.data.keyword.cloudant_short_notm}} features                                  |
| **Billing frequency**            | Hourly, prorated                                       |
| **Upgrade path**                 | Can scale up/down anytime                              |
{: caption="Summary of {{site.data.keyword.cloudantfull}} Standard plan" caption-side="top"}

Refer to [Usage and charges](/docs/cloudant-gen2?topic=cloudant-gen2-usage-and-charges){: external}
for more details on data storage and provisioned throughput capacity, including
how to estimate costs for the Standard plan.
{: tip}

## Provisioning a {{site.data.keyword.cloudant_short_notm}} instance
{: #provisioning-a-cloudant-nosql-db-instance-on-ibm-cloud}

Refer to [Getting started](/docs/cloudant-gen2?topic=cloudant-gen2-getting-started-with-cloudant) for provisioning instructions.

## Locations and tenancy
{: #locations-and-tenancy}

The Standard plan is deployed on multi-tenant
environments. As part of your plan selection, you can choose from the following {{site.data.keyword.cloud_notm}} locations:

- Frankfurt (eu-de)

Please refer to [IBM Cloud regions and data centers for high availability](/docs/overview?topic=overview-locations) page for more information on the configuration of the data centers in each location.

&Dagger;All {{site.data.keyword.cloudant_short_notm}} instances that are deployed from the
{{site.data.keyword.cloud_notm}} Frankfurt region
are deployed into EU-managed environments. Any {{site.data.keyword.cloudant_short_notm}}
account or API key that is generated
outside an EU-managed environment can't be granted access to an EU-managed
{{site.data.keyword.cloudant_short_notm}} instance. For more information, see [Enabling the EU Supported setting](/docs/account?topic=account-eu-supported) for your {{site.data.keyword.cloud_notm}} account.

## High availability, disaster recovery, and backup in a data center
{: #high-availability-disaster-recovery-and-backup-in-a-data-center}

To provide high availability (HA) and disaster recovery (DR) within a data center, all data is
stored in triplicate across three separate zones in a region. You can provision
accounts in multiple regions, then use continuous data replication to provide HA/DR across georgrahic areas. {{site.data.keyword.cloudant_short_notm}} data isn't automatically backed up, but supported tools are provided to handle backups. Review the
[Disaster recovery and backup guide](/docs/cloudant-gen2?topic=cloudant-gen2-disaster-recovery-and-backup)
to explore all HA, DR, and backup considerations to meet your application requirements.

## {{site.data.keyword.cloud_notm}} Support
{: #ibm-cloud-support}

Support for Standard plan service instances is optional.
Support is provided when you purchase *{{site.data.keyword.cloud_notm}} Standard Support*.

For more information, see the [{{site.data.keyword.cloud_notm}} Standard Support plans](https://www.ibm.com/cloud/support#944376){: external} and the [{{site.data.keyword.IBM_notm}} support guide](https://www.ibm.com/support/pages/node/733923){: external}.

The support systems that are used for {{site.data.keyword.cloudant_short_notm}} don't offer features for the protection of personal data or sensitive personal data. This content includes Healthcare Information, health data, Protected Health Information, or data that is subject to more regulatory requirements. As such, the Client must not enter or provide such data when interacting with {{site.data.keyword.cloudant_short_notm}} support.
{: note}
