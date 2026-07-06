---

copyright:
  years: 2020, 2026
lastupdated: "2026-06-22"

keywords:  api keys, enable iam, provisioning,  making requests, required client libraries, actions, endpoints, map actions to iam roles, manage credentials

subcollection: cloudant-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Managing access for {{site.data.keyword.cloudant_short_notm}}
{: #managing-access-for-cloudant}

{{site.data.keyword.cloud}} Identity and Access Management provides a unified
approach to managing user identities, services, and access control.
{: shortdesc}

The following text describes the integration of {{site.data.keyword.cloudantfull}} with {{site.data.keyword.cloud_notm}} Identity and
Access Management. The following topics are discussed:



- How to use IAM within {{site.data.keyword.cloudant_short_notm}}'s client libraries by using HTTP calls.
- Description of the IAM actions and roles available within {{site.data.keyword.cloudant_short_notm}}.


For more information, see an overview of [IAM](/docs/iam?topic=iam-iamoverview){: external} that includes the following topics:

- Manage user and service IDs.
- Manage available credentials.
- Use IAM access policies that allow and revoke access to {{site.data.keyword.cloudant_short_notm}} service instances.





## Policy enforcement
{: #iam-policy-enforcement}

IAM policies are enforced hierarchically from greatest level of access to most restricted, with more permissive overriding less permissive policies. For example, if a user has both the `Writer` and `Reader` service access role on a database, the policy granting the `Reader` role is ignored.

This is also applicable to service instance and database level policies.

- If a user has a policy granting the `Writer` role on a service instance and the `Reader` role on a single database, the database-level policy is ignored.
- If a user has a policy granting the `Reader` role on a service instance and the `Writer` role on a single database, both policies are enforced and the more permissive `Writer` role will take precedence for the individual database.

If it is necessary to restrict access to a single database (or set of databases), ensure that the user or Service ID doesn't have any other instance level policies by using either the console or CLI.

See [Best practices for organizing resources and assigning access](/docs/account?topic=account-account_setup) to learn more.






### Service credential JSON examples for each option
{: #service-credential-json-examples-for-each-option-ai}

 When you generate credentials within the primary {{site.data.keyword.cloud_notm}}
IAM interface, API keys are shown in that interface when generated.

You can also generate credentials from the Service Credentials section of a
service instance. Generating service credentials this way creates a service credentials JSON blob that can be pasted into applications with all the details that are needed to access the service instance.

Next, you can see what the service credential JSON looks like and what
each value means.



```json
{
  "apikey": "MxVp86XHkU82Wc97tdvDF8qM8B0Xdit2RqR1mGfVXPWz",
  "iam_apikey_description": "Auto generated apikey during resource-key [...]",
  "iam_apikey_id": "050d21b5-5f4a-4a5b-8a2c-0a2c0a2c0a2c",
  "iam_apikey_name": "auto-generated-apikey-050d21b5-5f[...]",
  "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
  "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::[...]",
  "url": "https://00000000-0000-0000-0000-00000000.abc.cloudant.eu-de.dataservices.appdomain.cloud",
  "vpe_url": "https://00000000-0000-0000-0000-00000000.private.abc.eu-de.cloudant.dataservices.appdomain.cloud"
}
```
{: codeblock}

Each value in the previous JSON example must be interpreted by using the following definitions:

`apikey`
:  IAM API key.

`iam_apikey_description`
:  Description of IAM API key.

`iam_apikey_id`
:  Unique id the of IAM API key.

`iam_apikey_name`
:  ID of IAM API key.

`iam_role_crn`
:  The IAM role that the IAM API key has.

`iam_serviceid_crn`
:  The CRN of service ID.

`url`
:  {{site.data.keyword.cloudant_short_notm}} public service URL.

`vpe_url`
:  {{site.data.keyword.cloudant_short_notm}} private service URL.




## Database-level IAM policies
{: #database-level-iam-policies}

IAM polices can be defined to restrict access to individual databases or those databases matching a wildcard pattern.

To target a database, set the attribute **Resource Type** to `database`. There are two operators
available:

| Operator         | Description                                                                                                                                                                 |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `string equals`  | matches a URL encoded database name exactly.                                                                                                                                |
| `string matches` | match using a multi-character wildcard (*), which matches any sequence of zero or more characters, a single-character wildcard (?), matching any single character, or both. |
{: caption="Database-level IAM operators" caption-side="top"}

Database names should be URL encoded in the **Resource ID** field of the policy, except for forward slashes `/`. This *does not* apply to any wildcard characters in the policy.

Note that leaving the **Resource Type** or **Resource ID** fields blank will create an instance-level policy.
{: tip}


### Examples
{: #database-level-iam-policies-examples}

| Description                        | Attribute         | Operator         | Value          |
|------------------------------------|-------------------|------------------|----------------|
| databases named `movies`           | **Resource Type** | `string equals`  | `database`     |
|                                    | **Resource ID**   | `string equals`  | `movies`       |
| databases starting with `movies`   | **Resource Type** | `string equals`  | `database`     |
|                                    | **Resource ID**   | `string matches` | `movies*`      |
| databases named `movies+new`       | **Resource Type** | `string equals`  | `database`     |
|                                    | **Resource ID**   | `string equals`  | `movies%2Bnew` |
| databases starting with `movies+*` | **Resource Type** | `string equals`  | `database`     |
|                                    | **Resource ID**   | `string matches` | `movies%2B*`   |
| databases named `movies/new`       | **Resource Type** | `string equals`  | `database`     |
|                                    | **Resource ID**   | `string equals`  | `movies/new`   |
{: caption="Database-level IAM operator examples" caption-side="top"}

## Create a replication job by using IAM credentials only
{: #create-replication-job-using-iam-cred-only-ai}

Follow these instructions to generate IAM API keys, generate the bearer token, create the `_replicator` database, and create the replication job.

### Generating IAM API keys for Source and Target and one for {{site.data.keyword.cloudant_short_notm}} API access
{: #generate-iam-api-keys-cloudant-api-access-ai}

In this exercise, the first two API keys are created so that the two instances can talk to each other during the replication process. The third API key is for the user to access the {{site.data.keyword.cloudant_short_notm}} API, create the `_replicator` database, and then add the replication document to it.

Follow these steps to generate IAM API keys and API access for {{site.data.keyword.cloudant_short_notm}}. You must write down
the credentials that are requested in the following steps to continue with the example.

Ensure that you select the specified instance, either the Source or Target.
{: note}

1. Log in to `cloud.ibm.com`.
2. From the Resource list, select **Services** and your Source instance.

    1. Click **Service credentials** and click **New credential**.

    2. Name the new credential `replicator-source`, and select the Manager role.

    3. Click **Add**, and make note of its `apikey`, which is under View Credentials in the Actions column.

3. Repeat steps 2 through 2.c. for the Target instance.

    1. Create a credential called `replicator-target` with the Manager role.

    2. Make note of its IAM API key, which is under View Credentials in the Actions column.

4. Select the Source instance, and click **Service credentials** and **New credential**.

    1. Name the new credential `apiaccess`, and select the Manager role.

    2. Make note of the actual IAM API key under View Credentials in the Actions column.

5. Make note of Source and Target instance URLs.

Depending on your workflow, instead of creating a service-level credential (step 4), you can use a personal IAM API key, as detailed in [Creating an API key](/docs/iam?topic=iam-userapikey&interface=ui#create_user_key){: external}.

You can also complete these steps on the command line by using the [{{site.data.keyword.cloud_notm}} CLI tool chain](/docs/cli?topic=cli-getting-started){: external}.

### Generating a bearer token to authenticate against the {{site.data.keyword.cloudant_short_notm}} API
{: #generate-bearer-token-authenticate-cloudant-api-ai}

In step 4.b., you wrote down the `apiaccess` key. Use that key now:

```sh
curl -k -X POST \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --header "Accept: application/json" \
  --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
  --data-urlencode "apikey=aSCsx4...2lN97h_2Ts" \
  "https://iam.cloud.ibm.com/identity/token"
```
{: codeblock}
{: curl}

The `apiaccess` key returns the following information (abbreviated):

```json
{
   "access_token": "eyJraWQiOiIyMDE5MD...tIwkCO9A",
   "refresh_token": "ReVbNrHo3UA38...mq67g",
   "token_type": "Bearer",
   "expires_in": 3600,
   "expiration": 1566313064,
   "scope": "ibm openid"
}
```
{: codeblock}

Create an environment variable to save some typing by using the value under the `access_token` key in the response data:

```sh
export TOK="Bearer eyJraWQiOiIyMDE5MD...tIwkCO9A"
```
{: codeblock}

### Creating the `_replicator` database on the Source side
{: #create-replicator-database-source-side-ai}

URL is the Source instance URL that you previously wrote down in step 4.b.

```sh
curl -k -X PUT \
     -H"Content-type: application/json" \
     -H'Authorization: '"$TOK"'' \
     'https://00000000-0000-0000-0000-00000000.abc.cloudant.eu-de.dataservices.appdomain.cloud"/_replicator'
```
{: codeblock}
{: curl}

See the results in the following example:

```json
{"ok": "true"}
```
{: codeblock}

### Creating the replication job
{: #create-the-replication-job-ai}

Create a file called `data.json` that contains the following information. The two keys are the Source and Target API keys that are created in the beginning, and the Source and Target instance URLs, with database names added.

```json
{
  "source": {
    "url": "https://00000000-0000-0000-0000-00000000.abc.cloudant.eu-de.dataservices.appdomain.cloud/source",
    "auth": {
      "iam": {
        "api_key": "xju1...TxuS"
      }
    }
  },
  "target": {
    "url": "https://00000000-0000-0000-0000-00000000.abc.cloudant.eu-de.dataservices.appdomain.cloud/target",
    "auth": {
      "iam": {
        "api_key": "UElc7...QIaL01Bjn"
      }
    }
  },
  "create_target": true,
  "continuous": true
}
```
{: codeblock}

Now, write a replication document called `source_dest` to the `_replicator` database on the Source instance.

```sh
curl -k -X PUT \
     -H"Content-type: application/json" \
     -H'Authorization: '"$TOK"'' \
     'https://00000000-0000-0000-0000-00000000.abc.cloudant.eu-de.dataservices.appdomain.cloud/_replicator/source_dest' -d@data.json
```
{: codeblock}
{: curl}

See the results in the following example:

```json
{"ok":true,"id":"source_dest","rev":"1-89b01e42968acd5944ed657b87c49f0c"}
```
{: codeblock}



## Making requests to instances by using IAM credentials
{: #making-requests-to-instances-by-using-iam-credentials-ai}

Now, the following section describes how to use {{site.data.keyword.cloudant_short_notm}} with
service instances through IAM authentication. It uses the
details from the [Service credential JSON examples for each option](#service-credential-json-examples-for-each-option-ai).

{{site.data.keyword.cloud_notm}} IAM requires that an IAM API key is exchanged for a time-limited access token before you make a request to a resource or service. The access token is then included in the `Authorization` HTTP header to the service. When the access token expires, the consuming application must handle getting a new one from the IAM token service. For more information, see [Getting an {{site.data.keyword.cloud_notm}} IAM token by using an API key](/docs/iam?topic=iam-iamtoken_from_apikey){: external} documentation for more details.

{{site.data.keyword.cloudant_short_notm}}'s official client libraries handle obtaining a token from an API key for you. You can access {{site.data.keyword.cloudant_short_notm}} directly by using an HTTP client rather than an {{site.data.keyword.cloudant_short_notm}} client library. However, you must handle exchanging and refreshing a time-limited access token by using an IAM API key with the IAM token service. After a token expires, {{site.data.keyword.cloudant_short_notm}} returns an HTTP `401` status code.

### Required client library versions
{: #required-client-library-versions-ai}

IAM connectivity is available in the latest release of all supported client libraries. For more information, see [Client libraries](/docs/cloudant-gen2?topic=cloudant-gen2-client-libraries).

#### Java
{: #java-ai}

The following link provides the latest supported version of the {{site.data.keyword.cloudant_short_notm}} Java&trade; library:

- [`cloudant-java-sdk`](https://github.com/IBM/cloudant-java-sdk/releases){: external}

For an example that uses {{site.data.keyword.cloudant_short_notm}} SDK for Java, see the [API and SDK documentation](/apidocs/cloudant?code=java#authentication){: external}.


#### Node.js
{: #node.js-ai}

The following link provides the latest supported version of the {{site.data.keyword.cloudant_short_notm}} Node.js library:

- [`cloudant-node-sdk`](https://github.com/IBM/cloudant-node-sdk/releases){: external}

For an example that uses {{site.data.keyword.cloudant_short_notm}} SDK for Node, see the [API and SDK documentation](/apidocs/cloudant?code=node#authentication){: external}.

#### Python
{: #python-ai}

The following link provides the latest supported version of the {{site.data.keyword.cloudant_short_notm}} Python library:

- [`cloudant-python-sdk`](https://github.com/IBM/cloudant-python-sdk/releases){: external}

For an example that uses {{site.data.keyword.cloudant_short_notm}} SDK for Python, see the [API and SDK documentation](/apidocs/cloudant?code=python#authentication){: external}.

#### Go
{: #go-ai}

The following link provides the latest supported version of the {{site.data.keyword.cloudant_short_notm}} Go library:

- [`go-sdk`](https://github.com/IBM/cloudant-go-sdk/releases){: external}

For an example that uses {{site.data.keyword.cloudant_short_notm}} SDK for Go, see the [API and SDK documentation](/apidocs/cloudant?code=go#authentication){: external}.




## Roles and actions
{: #reference-ai}

The following tables include a complete list of {{site.data.keyword.cloudant_short_notm}}'s IAM roles and actions, and a mapping of what actions are allowed for each IAM system role.

### {{site.data.keyword.cloudant_short_notm}} roles
{: #ibm-cloudant-roles-ai}

The following table lists the available IAM service roles for {{site.data.keyword.cloudant_short_notm}} and a brief description of each.

| Role | Description |
|--------|----------|
| `Manager` | Includes the ability to access all endpoints and perform all administrative functions on an instance, such as creating databases, changing capacity, reading and writing data and indexes, and accessing the Dashboard. |
| `Writer` | Includes the ability to read and write to all databases and documents, but not able to create indexes. |
| `Reader` | Includes the ability to read all databases and documents, but not able to write new documents or create indexes. |
| `Monitor` | Includes the ability to read monitoring endpoints, such as  `_active_tasks`  and replication  `_scheduler` endpoints. |
| `Checkpointer` | Includes the ability to write replication `checkpointer` `_local` documents. Required on source databases during replication. |
{: caption="IAM service roles for {{site.data.keyword.cloudant_short_notm}}" caption-side="top"}

Manager is inclusive of all actions of Reader and Writer, and Writer is inclusive of all actions of Reader.

### {{site.data.keyword.cloudant_short_notm}} actions
{: #ibm-cloudant-actions-ai}

The following table describes the available IAM actions and roles. For fine-grained authorization, you can use the roles of `Manager`, `Reader`, `Writer`, `Monitor`, or `Checkpointer`.



| Method | Endpoint | Action name |
|--------|----------|-------------|
| `GET/HEAD` | / | `cloudantnosqldb.account-meta-info.read` |
| `GET/HEAD` | `/_active_tasks` | `cloudantnosqldb.account-active-tasks.read` |
| `GET/HEAD` | `/_replicator` | `cloudantnosqldb.replicator-database-info.read` |
| `GET/HEAD` | `/_replicator/$DOCUMENT` | `cloudantnosqldb.replication.read` |
| `GET/HEAD` | `/_scheduler/jobs` | `cloudantnosqldb.replication-scheduler.read` |
| `GET/HEAD` | `/_scheduler/docs` | `cloudantnosqldb.replication-scheduler.read` |
| `POST` | `/_replicate` | `cloudantnosqldb.replication.write` |
| `POST` | `/_replicator` | `cloudantnosqldb.replication.write` |
| `PUT/DELETE` | `/_replicator` | `cloudantnosqldb.replicator-database.create` |
| `PUT/DELETE` | `/_replicator/$DOCUMENT` | `cloudantnosqldb.replication.write` |
| `GET/HEAD` | `/_up` | `cloudantnosqldb.account-up.read` |
| `PUT` | `/$DATABASE/` | `cloudantnosqldb.database.create` |
| `DELETE` | `/$DATABASE` | `cloudantnosqldb.database.delete` |
| `POST` | `/$DATABASE/_design_docs/queries` | `cloudantnosqldb.any-document.read` |
| `GET/HEAD` | `/$DATABASE/_design/$DOCUMENT_ID/_geo_info` | `cloudantnosqldb.any-document.read` |
| `GET/HEAD` | `/$DATABASE/_design/$DOCUMENT_ID/_info/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `GET` | `/$DATABASE/_design/$DOCUMENT_ID/_search_disk_size/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `GET` | `/$DATABASE/_design/$DOCUMENT_ID/_search_info/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `GET/HEAD` | `/$DATABASE/_index/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `GET` | `/$DATABASE/_design_docs`  | `cloudantnosqldb.any-document.read` |
| `GET` | `/$DATABASE/_design/$DOCUMENT_ID` | `cloudantnosqldb.any-document.read` |
| `GET/HEAD` | `/$DATABASE/_design/$DOCUMENT_ID/$ATTACHMENT` | `cloudantnosqldb.any-document.read` |
| `PUT` | `/$DATABASE/_design/$DOCUMENT_ID` | `cloudantnosqldb.design-document.write` |
| `COPY` | `/$DATABASE/_design/$DOCUMENT_ID` | `cloudantnosqldb.design-document.write` |
| `DELETE` | `/$DATABASE/_design/$DOCUMENT_ID` | `cloudantnosqldb.design-document.write` |
| `PUT` | `/$DATABASE/_design/$DOCUMENT_ID/$ATTACHMENT` | `cloudantnosqldb.design-document.write` |
| `DELETE` | `/$DATABASE/_design/$DOCUMENT_ID/$ATTACHMENT` | `cloudantnosqldb.design-document.write` |
| `POST/DELETE` | `/$DATABASE/_index/$FURTHER_PATH_PARTS` | `cloudantnosqldb.design-document.write` |
| `GET/HEAD` | `/$DATABASE/_security` | `cloudantnosqldb.database-security.read` |
| `PUT` | `/$DATABASE/_security` | `cloudantnosqldb.database-security.write` |
| `GET/HEAD` | `/$DATABASE/_shards` | `cloudantnosqldb.database-shards.read` |
| `COPY`    (Depends on write document type.) | `/$DATABASE/$DOCUMENT_ID` | `cloudantnosqldb.any-document.read` +   `cloudantnosqldb.design-document.write` either or both `cloudantnosqldb.local-document.write` either or both `cloudantnosqldb.data-document.write` |
| `GET` | `/_membership` | `cloudantnosqldb.cluster-membership.read` |
| `POST` | `/$DATABASE/_ensure_full_commit` | `cloudantnosqldb.database-ensure-full-commit.execute` |
| `PUT` | `/_users` | `cloudantnosqldb.users-database.create`  |
| `GET/HEAD` | `/_users` | `cloudantnosqldb.users-database-info.read`  |
| `DELETE` | `/_users` | `cloudantnosqldb.users-database.delete`  |
| `GET/HEAD` | `/_users/$DOCUMENT` | `cloudantnosqldb.users.read` |
| `GET/POST` | `/_users/_all_docs` | `cloudantnosqldb.users.read` |
| `GET/POST` | `/_users/_changes` | `cloudantnosqldb.users.read` |
| `POST` | `/_users/_missing_revs` | `cloudantnosqldb.users.read` |
| `POST` | `/_users/_revs_diff` | `cloudantnosqldb.users.read` |
| `POST` | `/_users/_bulk_get` | `cloudantnosqldb.users.read` |
| `PUT/DELETE` | `/_users/$DOCUMENT` | `cloudantnosqldb.users.write` |
| `POST` | `/_users/_bulk_docs` | `cloudantnosqldb.users.write` |
| `POST` | `/_users/` | `cloudantnosqldb.users.write` |
| `GET/HEAD` | `/_uuids` | `cloudantnosqldb.cluster-uuids.execute` |
| `POST` | `/$DATABASE/` | `cloudantnosqldb.data-document.write` or `cloudantnosqldb.design-document.write` or `cloudantnosqldb.local-document.write` |
| `POST` | `/$DATABASE/_bulk_docs` | `cloudantnosqldb.data-document.write` either or both `cloudantnosqldb.design-document.write` either or both `cloudantnosqldb.local-document.write` |
| `PUT` | `/$DATABASE/$DOCUMENT_ID` | `cloudantnosqldb.data-document.write` |
| `DELETE` | `/$DATABASE/$DOCUMENT_ID` | `cloudantnosqldb.data-document.write` |
| `PUT` | `/$DATABASE/$DOCUMENT_ID/$ATTACHMENT` | `cloudantnosqldb.data-document.write` |
| `DELETE` | `/$DATABASE/$DOCUMENT_ID/$ATTACHMENT` | `cloudantnosqldb.data-document.write` |
| `PUT/DELETE` | `/$DATABASE/_local/$DOCUMENT_ID` | `cloudantnosqldb.local-document.write` |
| `COPY`    (Depends on write document type.) | `/$DATABASE/_local/$DOCUMENT_ID` | `cloudantnosqldb.any-document.read` + `cloudantnosqldb.design-document.write` either or both `cloudantnosqldb.local-document.write` either or both `cloudantnosqldb.data-document.write` |
| `GET/HEAD` | `/_iam_session` | `cloudantnosqldb.iam-session.read` |
| `POST` | `/_iam_session` | `cloudantnosqldb.iam-session.write` |
| `DELETE` | `/_iam_session` | `cloudantnosqldb.iam-session.delete` |
| `GET/HEAD` | `/_session` | `cloudantnosqldb.session.read` |
| `POST` | `/_session` | `cloudantnosqldb.session.write` |
| `DELETE` | `/_session` | `cloudantnosqldb.session.delete` |
| `GET/HEAD` | `/_all_dbs` | `cloudantnosqldb.account-all-dbs.read` |
| `POST` | `/_dbs_info` | `cloudantnosqldb.account-dbs-info.read` |
| `GET` | `/$DATABASE/` | `cloudantnosqldb.database-info.read` |
| `GET/POST` | `/$DATABASE/_all_docs` | `cloudantnosqldb.any-document.read` |
| `GET/POST` | `/$DATABASE/_changes` | `cloudantnosqldb.any-document.read` |
| `GET/HEAD` | `/$DATABASE/$DOCUMENT_ID` | `cloudantnosqldb.any-document.read` |
| `GET/HEAD` | `/$DATABASE/$DOCUMENT_ID/$ATTACHMENT` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_bulk_get` | `cloudantnosqldb.any-document.read` |
| `GET/POST` | `/_search_analyze` | `cloudantnosqldb.account-search-analyze.execute` |
| `POST` | `/$DATABASE/_all_docs/queries` | `cloudantnosqldb.any-document.read` |
| `GET/HEAD` | `/$DATABASE/_design/$DOCUMENT_ID/_geo/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `GET/POST` | `/$DATABASE/_design/$DOCUMENT_ID/_search/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_design/$DOCUMENT_ID/_view/$VIEW/queries` | `cloudantnosqldb.any-document.read` |
| `GET/POST` | `/$DATABASE/_design/$DOCUMENT_ID/_view/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_explain/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_find/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `GET` | `/$DATABASE/_local/$DOCUMENT_ID` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_missing_revs` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_revs_diff` | `cloudantnosqldb.any-document.read` |
{: class="simple-tab-table"}
{: caption="Actions and mapping for the Manager role" caption-side="top"}
{: #manager-role}
{: tab-title="Manager"}
{: tab-group="Roles-simple"}




| Method | Endpoint | Action name |
|--------|----------|-------------|
| `GET/HEAD` | `/_uuids` | `cloudantnosqldb.cluster-uuids.execute` |
| `POST` | `/$DATABASE/` | `cloudantnosqldb.data-document.write` or `cloudantnosqldb.design-document.write` or `cloudantnosqldb.local-document.write` |
| `POST` | `/$DATABASE/_bulk_docs` | `cloudantnosqldb.data-document.write` either or both `cloudantnosqldb.design-document.write` either or both `cloudantnosqldb.local-document.write` |
| `PUT` | `/$DATABASE/$DOCUMENT_ID` | `cloudantnosqldb.data-document.write` |
| `DELETE` | `/$DATABASE/$DOCUMENT_ID` | `cloudantnosqldb.data-document.write` |
| `PUT` | `/$DATABASE/$DOCUMENT_ID/$ATTACHMENT` | `cloudantnosqldb.data-document.write` |
| `DELETE` | `/$DATABASE/$DOCUMENT_ID/$ATTACHMENT` | `cloudantnosqldb.data-document.write` |
| `PUT/DELETE` | `/$DATABASE/_local/$DOCUMENT_ID` | `cloudantnosqldb.local-document.write` |
| `GET/HEAD` | `/_iam_session` | `cloudantnosqldb.iam-session.read` |
| `POST` | `/_iam_session` | `cloudantnosqldb.iam-session.write` |
| `DELETE` | `/_iam_session` | `cloudantnosqldb.iam-session.delete` |
| `GET/HEAD` | `/_session` | `cloudantnosqldb.session.read` |
| `POST` | `/_session` | `cloudantnosqldb.session.write` |
| `DELETE` | `/_session` | `cloudantnosqldb.session.delete` |
| `GET/HEAD` | `/_all_dbs` | `cloudantnosqldb.account-all-dbs.read` |
| `POST` | `/_dbs_info` | `cloudantnosqldb.account-dbs-info.read` |
| `GET` | `/$DATABASE/` | `cloudantnosqldb.database-info.read` |
| `GET/POST` | `/$DATABASE/_all_docs` | `cloudantnosqldb.any-document.read` |
| `GET/POST` | `/$DATABASE/_changes` | `cloudantnosqldb.any-document.read` |
| `GET/HEAD` | `/$DATABASE/$DOCUMENT_ID` | `cloudantnosqldb.any-document.read` |
| `GET/HEAD` | `/$DATABASE/$DOCUMENT_ID/$ATTACHMENT` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_bulk_get` | `cloudantnosqldb.any-document.read` |
| `GET/POST` | `/_search_analyze` | `cloudantnosqldb.account-search-analyze.execute` |
| `POST` | `/$DATABASE/_all_docs/queries` | `cloudantnosqldb.any-document.read` |
| `GET/HEAD` | `/$DATABASE/_design/$DOCUMENT_ID/_geo/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `GET/POST` | `/$DATABASE/_design/$DOCUMENT_ID/_search/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_design/$DOCUMENT_ID/_view/$VIEW/queries` | `cloudantnosqldb.any-document.read` |
| `GET/POST` | `/$DATABASE/_design/$DOCUMENT_ID/_view/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_explain/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_find/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `GET` | `/$DATABASE/_local/$DOCUMENT_ID` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_missing_revs` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_revs_diff` | `cloudantnosqldb.any-document.read` |
| `GET/HEAD` | / | `cloudantnosqldb.account-meta-info.read` |
| `POST` | `/$DATABASE/_ensure_full_commit` | `cloudantnosqldb.database-ensure-full-commit.execute` |
{: caption="Actions and mapping for the Writer role" caption-side="top"}
{: #writer-role}
{: tab-title="Writer"}
{: tab-group="Roles-simple"}
{: class="simple-tab-table"}



| Method | Endpoint | Action name |
|--------|----------|-------------|
| `GET/HEAD` | `/_iam_session` | `cloudantnosqldb.iam-session.read` |
| `POST` | `/_iam_session` | `cloudantnosqldb.iam-session.write` |
| `DELETE` | `/_iam_session` | `cloudantnosqldb.iam-session.delete` |
| `GET/HEAD` | `/_session` | `cloudantnosqldb.session.read` |
| `POST` | `/_session` | `cloudantnosqldb.session.write` |
| `DELETE` | `/_session` | `cloudantnosqldb.session.delete` |
| `GET/HEAD` | `/_all_dbs` | `cloudantnosqldb.account-all-dbs.read` |
| `POST` | `/_dbs_info` | `cloudantnosqldb.account-dbs-info.read` |
| `GET` | `/$DATABASE/` | `cloudantnosqldb.database-info.read` |
| `GET/POST` | `/$DATABASE/_all_docs` | `cloudantnosqldb.any-document.read` |
| `GET/POST` | `/$DATABASE/_changes` | `cloudantnosqldb.any-document.read` |
| `GET/HEAD` | `/$DATABASE/$DOCUMENT_ID` | `cloudantnosqldb.any-document.read` |
| `GET/HEAD` | `/$DATABASE/$DOCUMENT_ID/$ATTACHMENT` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_bulk_get` | `cloudantnosqldb.any-document.read` |
| `GET/POST` | `/_search_analyze` | `cloudantnosqldb.account-search-analyze.execute` |
| `POST` | `/$DATABASE/_all_docs/queries` | `cloudantnosqldb.any-document.read` |
| `GET/HEAD` | `/$DATABASE/_design/$DOCUMENT_ID/_geo/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `GET/POST` | `/$DATABASE/_design/$DOCUMENT_ID/_search/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_design/$DOCUMENT_ID/_view/$VIEW/queries` | `cloudantnosqldb.any-document.read` |
| `GET/POST` | `/$DATABASE/_design/$DOCUMENT_ID/_view/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_explain/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_find/$FURTHER_PATH_PARTS` | `cloudantnosqldb.any-document.read` |
| `GET` | `/$DATABASE/_local/$DOCUMENT_ID` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_missing_revs` | `cloudantnosqldb.any-document.read` |
| `POST` | `/$DATABASE/_revs_diff` | `cloudantnosqldb.any-document.read` |
| `GET/HEAD` | / | `cloudantnosqldb.account-meta-info.read` |
{: caption="Actions and mapping for the Reader role" caption-side="top"}
{: #reader-role}
{: tab-title="Reader"}
{: tab-group="Roles-simple"}
{: class="simple-tab-table"}



| Method | Endpoint | Action name |
|--------|----------|-------------|
| `GET/HEAD` | / | `cloudantnosqldb.account-meta-info.read` |
| `GET/HEAD` | `/_active_tasks` | `cloudantnosqldb.account-active-tasks.read` |
| `GET/HEAD` | `/_scheduler/jobs` | `cloudantnosqldb.replication-scheduler.read` |
| `GET/HEAD` | `/_scheduler/docs` | `cloudantnosqldb.replication-scheduler.read` |
| `GET/HEAD` | `/_up` | `cloudantnosqldb.account-up.read` |
| `GET/HEAD` | `/$DATABASE/_shards` | `cloudantnosqldb.database-shards.read` |
| `PUT/DELETE` | `/$DATABASE/_local/$DOCUMENT_ID` | `cloudantnosqldb.local-document.write` |
| `POST` | `/_dbs_info` | `cloudantnosqldb.account-dbs-info.read` |
| `GET` | `/$DATABASE/` | `cloudantnosqldb.database-info.read` |
{: caption="Actions and mapping for the Monitor role" caption-side="top"}
{: #monitor-role}
{: tab-title="Monitor"}
{: tab-group="Roles-simple"}
{: class="simple-tab-table"}



| Method | Endpoint | Action name |
|--------|----------|-------------|
| `PUT/DELETE` | `/$DATABASE/_local/$DOCUMENT_ID` | `cloudantnosqldb.local-document.write` |
{: caption="Actions and mapping for the Checkpointer role" caption-side="top"}
{: #checkpointer-role}
{: tab-title="Checkpointer"}
{: tab-group="Roles-simple"}
{: class="simple-tab-table"}

#### Unavailable endpoints
{: #unavailable-endpoints-ai}

The following endpoints are not available to requests authorized with IAM:

- HTTP rewrite handlers - `/db/_design/design-doc/_rewrite/path`.

While design documents can contain rewrite handlers, users cannot call them.

- Update handlers - `POST /db/_design/ddoc/_update/func`.

While design documents can contain update functions, users cannot call them.

## Troubleshooting
{: #troubleshooting-ai}

If you can't use IAM to authenticate when you make requests to your {{site.data.keyword.cloudant_short_notm}} service instance, verify your account as shown in the next section.

### Ensure that your account is IAM enabled
{: #ensure-your-account-is-iam-enabled-ai}

On the Overview portion of the {{site.data.keyword.cloudant_short_notm}} Dashboard, "authentication method" is listed under deployment details. Your available authentication methods are listed there.
