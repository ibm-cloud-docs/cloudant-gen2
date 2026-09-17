---
copyright:
  years: 2026
lastupdated: "2026-09-17"

keywords: cloudant gen 2 overview

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Overview of Gen 1 (Classic) and Gen 2 (VPC)
{: #overview-gen1-gen2}

[Gen 2]{: tag-purple}

This page outlines the key differences between {{site.data.keyword.cloudant_short_notm}} built on IBM’s Classic platform (Gen 1) and the latest VPC based platform (Gen 2).

## Generation  1 (Classic)
{: #gen1}

IBM’s original platform consisting of all databases across all regions and a rich feature set. Gen 1 Databases support both private and public endpoints, with options for isolated and shared compute. This environment is best suited for workloads that benefit from simpler networking and isolation features.

## Generation 2 (VPC)
{: #gen2}

Gen 2 databases are built on {{site.data.keyword.cloud}}’s latest platform, based on highly secure software-defined networking architecture and ideal for cloud-native applications. Gen 2 Databases currently are only available in select regions and support public and private endpoints. This environment is ideal for modern applications that demand advanced networking and secure, software-defined isolation.

## Feature differentiators
{: #feature-differentiators}

| Category                     | Gen 1                                                            | Gen 2                                             |
|-----------------------------|-------------------------------------------------------------------|---------------------------------------------------|
| Regions                     | Dallas (us-south) <br> Sao Paulo (br-sao) <br> Toronto (ca-tor) <br> Washington (us-east) <br> Frankfurt (eu-de) <br> London (eu-gb) <br> Madrid (eu-es) <br> Osaka (jp-osa) <br> Sydney (au-syd) <br> Tokyo (jp-tok) | Dallas (us-south) <br> Washington (us-east) <br> Frankfurt (eu-de) <br> London (eu-gb) <br> Madrid (eu-es) <br> Sydney (au-syd)  |
| Database editions           | Cloudant Standard <br> Cloudant Standard Dedicated | Cloudant Standard  |
| Endpoints                   |Public endpoints <br>  Private endpoints (dedicated-only)                          | Public endpoints <br> Private endpoints                                |
| Hosting models              | Multi-tenant <br> Dedicated  | Multi-tenant                                 |
{: caption="Feature differentiators" caption-side="bottom"}

## Performance differentiators
{: #performance-differentiators}

| Category               | Gen 1                                                                 | Gen 2                                               |
|------------------------|-----------------------------------------------------------------------|-----------------------------------------------------|
| Compute generation     | IBM Classic infrastructure                                            | IBM Cloud VPC                                       |
| Availability           | High availability                                                     | High availability                                   |
| Deployment timeframe   | Minutes                                                               | Minutes                                             |
| Pricing                | Hourly and monthly billing                                            | Hourly and monthly billing                          |
| Backup and restore     | Timing depends on size of backup and performance impact during backup | Timing depends on size of backup and performance impact during backup |
{: caption="Performance differentiators" caption-side="bottom"}

## Access, compliance, and security differentiators
{: #access-compliance-security-differentiators}

| Category               | Gen 1                                                                 | Gen 2                                              |
|------------------------|-----------------------------------------------------------------------|----------------------------------------------------|
| User and role management | [Database `admin` user created by IBM](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-user-management&interface=ui) | Database "manager" via service-credential                              |
| Certificate type       | Signed by DigiCert Inc                   | Certificates signed by Let's Encrypt         |
| Encryption             | Encryption at Rest <br> Encryption in Transit <br> Customer-managed encryption - Bring your own key (BYOK) | Encryption at Rest <br> Encryption in Transit |
{: caption="Access, compliance, and security differentiators" caption-side="bottom"}
