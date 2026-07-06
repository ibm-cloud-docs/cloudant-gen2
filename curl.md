---

copyright:
  years: 2015, 2024
lastupdated: "2024-11-15"

keywords: encode username, encode password, create alias, activate alias, test acurl, acurl

subcollection: Cloudant

---

{{site.data.keyword.attribute-definition-list}}

# Working with `curl`
{: #working-with-curl}

To simplify secure interaction with {{site.data.keyword.cloudant_short_notm}}, we suggest creating
an `acurl` alias to `curl`. This alias automatically sends your
{{site.data.keyword.cloudantfull}} credentials when making database HTTP
requests, without exposing them in your terminal history or needing them to be
typed in for every request.
{: shortdesc}

You use `curl` examples by following these steps.

## Simplifying using IAM credentials with `curl`
{: #working-with-iam-credentials}

Using `curl` with {{site.data.keyword.cloudant_short_notm}} accounts that use IAM for authentication can be
frustrating because API keys need to be exchanged for short-lived tokens that
are sent with requests.

A {{site.data.keyword.cloudant_short_notm}} engineer created the `ccurl` tool to help with this. For more information, see [ccurl on npm](https://www.npmjs.com/package/ccurl).
