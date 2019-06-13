---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-13"

keywords: Centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}


# Custom findings
{: #setup_custom}

With {{site.data.keyword.security-advisor_short}}, you can integrate your existing custom security tools, whether open source, custom developed, or third-party services. By integrating third-party findings, you're able to bring all of your security tools and findings in to one dashboard where you can monitor critical security events.
{: shortdesc}


## Before you begin
{: #custom-before-api}

Before you integrate findings from your third-party tool, be sure that you have the following prerequisites.

1. Be sure that the user or service ID that you're using is assigned the **Manager** [IAM role](https://cloud.ibm.com/iam#/users).

2. Log in to {{site.data.keyword.cloud_notm}}.

  ```
  ibmcloud login
  ```
  {: codeblock}

3. Get your account ID. For more information about service roles, check out the [{{site.data.keyword.security-advisor_short}} access policies](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

4. Get your Identity and Access Management (IAM) token. The token is used in the `--header` of each API request.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  IAM tokens expire every 60 minutes. To learn how to [get a new token automatically](/docs/iam?topic=iam-iamtoken_from_apikey) by using an API key.
  {: tip}



## Importing findings and KRIs
{: #custom-adding}

The APIs follow Grafeas like artifact metadata specifications to store, query, and retrieve the critical metadata for the findings that are reported by your security tools and services. The integration can be done by using the APIs to configure key risk indicators (KRIs).
{: shortdesc}


### Step 1: Registering a new finding type
{: #custom-register-finding}

To register a new type of findings, you can create a note. To create the note, you can use the [Findings API](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}. Be sure that you choose a unique provider ID to identify your custom tool. If you're automating the process by using your service ID as your provider ID.

Example request:

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{  \"kind\": \"FINDING\",  \"short_description\": \"My security tool finding\",  \"long_description\": \"Longer description of what the security tool found.\",  \"provder_id\": \"my-custom-tool\",  \"id\": \"my-custom-tool-findings-type\",  \"reported_by\": {    \"id\": \"my-custom-tool\",    \"title\": \"My custom security tool\"  } ,  \"finding\": {    \"severity\": \"MEDIUM\",    \"next_steps\": [      {        \"title\": \"Explain why it's reported as a risk.\"      }    ]  }}"
```
{: codeblock}

| General | Description | 
|:-----------------|:-----------------|
| `note_name` | `<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type`  |
| `kind` | `FINDING` |
| `remediation` | The steps that need to be taken to resolve the issue. |
| `provider_id` | Your custom security tool. |
| `id` | An ID for the type of finding that your security tool found. |
{: class="simple-tab-table"}
{: caption="Table 1. Understanding the command's general components" caption-side="top"}
{: #registerfindingtable1}
{: tab-title="General"}
{: tab-group="register"}

| Context | Description | 
|:-----------------|:-----------------|
| `region` | The location in which the finding occured.  |
| `resource_id` | The ID for the resource. |
| `resource_name` | The name of the resource. |
| `resource_type` | The type of resource. |
| `service_name` | The name of the service. |
{: class="simple-tab-table"}
{: caption="Table 2. Understanding the command's context components" caption-side="top"}
{: #registerfinding2}
{: tab-title="Context"}
{: tab-group="register"}

| Finding | Description | 
|:-----------------|:-----------------|
| `severity` | The level of urgency that the finding presents.  |
| `next_steps` | The steps that can be taken to remediate the issue. |
| `url` | A URL where the details of the finding can be found. |
{: class="simple-tab-table"}
{: caption="Table 3. Understanding the command's finding components" caption-side="top"}
{: #registerfinding3}
{: tab-title="Finding"}
{: tab-group="register"}

Example response:

```
{
  "author": {
    "account_id": "account id",
    "email": "email id",
    "id": "IBM ID",
    "kind": "user"
  },
  "context": {
    "account_id": "account id"
  },
  "create_time": "2018-09-04T10:57:56.913871Z",
  "create_timestamp": 1536058676914,
  "finding": {
    "next_steps": [
      {
        "title": "Learn why this is reported as a risk"
      }
    ],
    "severity": "MEDIUM"
  },
  "id": "my-custom-tool-findings-type",
  "kind": "FINDING",
  "long_description": "See what my custom security tool found",
  "name": "<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
  "provider_id": "my-custom-tool",
  "provider_name": "<account id>/providers/my-custom-tool",
  "reported_by": {
    "id": "my-custom-tool",
    "title": "My Custom Security Tool"
  },
  "shared": true,
  "short_description": "My security tool finding",
  "update_time": "2018-09-04T10:57:56.913890Z",
  "update_timestamp": 1536058676914,
  "update_week_date": "2018-W36-2"
}
```
{: screen}

Be sure to remember the name of the note that is returned as part of the response. In this example, the value is `/providers/my-custom-tool/notes/my-custom-tool-findings-type`. This value is used in the next step.
{: tip}


### Step 2: Posting findings
{: #custom-post-findings}

Create an [occurrence](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence){: external} to post findings as KRIs or events to your security advisor dashboard.

For each card, you can define two KRIs.
{: note}

Example request: 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/occurrences"  -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"note_name\": \"<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\",    \"kind\": \"FINDING\",    \"remediation\": \"How to resolve Data leakage threat\",    \"provider_id\": \"my-custom-tool\",    \"id\": \"my-custom-tool-finding-2\",    \"context\": {        \"region\": \"location\",        \"resource_id\": \"cluster crn\",        \"resource_name\": \"cloudkingdom\",        \"resource_type\": \"container\",        \"service_name\": \"kubernetes service\"    },    \"finding\": {        \"severity\": \"HIGH\",        \"next_steps\": [{            \"title\": \"Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities\",                        \"url\":\"https://cloud.ibm.com/containers-kubernetes/clusters\"        }],                \"short_description\": \"One of the pods in your cluster appears to be leaking an excessive amount of data\",                \"long_description\": \"One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod's normal behavior\"    }}"
```
{: codeblock}

| General | Description | 
|:-----------------|:-----------------|
| `kind` | `FINDING` |
| `short_description` | A short description that summaries the finding; no more than a couple of words. |
| `long_description` | A longer description that contains more details about the finding. |
| `provider_id` | Your custom security tool. |
| `id` | An ID for the type of finding that your security tool found. |
{: class="simple-tab-table"}
{: caption="Table 2. Understanding the command's general components" caption-side="top"}
{: #postfinding1}
{: tab-title="General"}
{: tab-group="post"}

| Reported by | Description | 
|:-----------------|:-----------------|
| `id` | The ID of the security tool that reported the finding.  |
| `title` | The title of the security tool that reported the finding. |
{: class="simple-tab-table"}
{: caption="Table 2. Understanding the command's reported by components" caption-side="top"}
{: #postfinding2}
{: tab-title="Reported by"}
{: tab-group="post"}

| Finding | Description | 
|:-----------------|:-----------------|
| `severity` | The level of urgency that the finding presents.  |
| `next_steps` | The steps that can be taken to remediate the issue. |
| `title` | The title of the finding. |
{: class="simple-tab-table"}
{: caption="Table 3. Understanding the command's finding components" caption-side="top"}
{: #postfinding3}
{: tab-title="Finding"}
{: tab-group="post"}


Example response:

```
  {
  "author": {
    "account_id": "account id",
    "email": "email id ",
    "id": "user id",
    "kind": "user"
  },
  "context": {
    "account_id": "account id",
    "region": "location",
    "resource_id": "pluginId",
    "resource_name": "www.myapp.com",
    "resource_type": "worker",
    "service_name": "application"
  },
  "create_time": "2018-09-04T11:32:14.564809Z",
  "create_timestamp": 1536060734565,
  "finding": {
    "next_steps": [
      {
        "title": "Learn why this is reported as a risk",
        "url": "Details URL"
      }
    ],
    "severity": "HIGH"
  },
  "id": "my-custom-tool-finding-1",
  "kind": "FINDING",
  "long_description": "See what my custom security tool found",
  "name": "<account id>/providers/my-custom-tool/occurrences/my-custom-tool-finding-1",
  "note_name": "<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
  "provider_id": "my-custom-tool",
  "provider_name": "<account id>/providers/my-custom-tool",
  "remediation": "how to resolve this",
  "reported_by": {
    "id": "my-custom-tool",
    "title": "My Custom Security Tool"
  },
  "short_description": "My security tool finding",
  "update_time": "2018-09-04T11:32:14.564828Z",
  "update_timestamp": 1536060734565,
  "update_week_date": "2018-W36-2"
}
```
{: screen}

### Step 3: Defining the card to display
{: #custom-define-card}

Define how you want your card to display your findings in your dashboard by creating a [note](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}.

Example request: 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"kind\": \"CARD\",        \"provider_id\": \"my-custom-tool\",    \"id\": \"custom-tool-card\",    \"short_description\": \"Security risk found by my custom tool\",        \"long_description\": \"More detailed description about why this security risk needs to be fixed\",    \"reported_by\": {      \"id\": \"my-custom-tool\",      \"title\": \"My security tool\"    },        \"card\": {      \"section\": \"My security tools\",      \"order\": 1 ,     \"title\": \"My security tool findings\",      \"subtitle\": \"My security tool\",      \"finding_note_names\": [        \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"      ],      \"elements\": [        {          \"kind\": \"NUMERIC\",          \"text\": \"Count of findings reported by my security tool\",          \"default_time_range\": \"1d\",          \"value_type\": {            \"kind\": \"FINDING_COUNT\",            \"finding_note_names\": [              \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"            ]          }        }      ]    }  }"
```
{: codeblock}

| General | Description | 
|:-----------------|:-----------------|
| `provider_id` | The ID of your security tool. |
| `id` | An ID for the type of finding that your security tool found. |
| `short_description` | A short description that summaries the finding; no more than a couple of words. |
| `long_description` | A longer description that contains more details about the finding. |
{: class="simple-tab-table"}
{: caption="Table 1. Understanding the command's general components" caption-side="top"}
{: #definecard1}
{: tab-title="General"}
{: tab-group="card"}

| Reported by | Description | 
|:-----------------|:-----------------|
| `id` | The ID of the security tool that reported the finding.  |
| `title` | The title of the security tool that reported the finding. |
{: class="simple-tab-table"}
{: caption="Table 2. Understanding the command's reported by components" caption-side="top"}
{: #definecard2}
{: tab-title="Reported by"}
{: tab-group="card"}

| Card | Description | 
|:-----------------|:-----------------|
| `section` | The section in which you want the card to display. You can have up to three custom sections with six cards in each section. Maximum characters: 30  |
| Optional: `order` | The order in which the card is displayed within the specified section. The order is specified in range 1 - 6. If you choose a number that is already applied to another card, the creation fails. You receive an error message that states "Given order is already taken by other card in section." If the order provided is greater than the current number of cards plus 1, then card creation fails. For example, if you currently have two cards and are creating another, you could not specify 5 in the card order because all together, you have three cards total. If the order for the cards is not specified, they are arranged alphabetically in the assigned section. |
| `title` | The title that you want your card to have. Maximum characters: 28 |
| `subtitle` | The subtitle that you want your card to have. Maximum characters: 30 |
| `finding_note_names` | `providers//notes/my-custom-tool-findings-type` |
{: class="simple-tab-table"}
{: caption="Table 3. Understanding the command's card components" caption-side="top"}
{: #definecard3}
{: tab-title="Card"}
{: tab-group="card"}

| Elements: Value type | Description | 
|:-----------------|:-----------------|
| `kind` | Options include: `NUMERIC`, `TIME_SERIES`, and `BREAKDOWN`. |
| `text` | The text that you want to display. If kind is `NUMERIC`, the maximum number of characters is 60. If kind is `TIME_SERIES` or `BREAKDOWN`, the maximum number of characters is 65. |
| `default_time_range` | The amount of time that you want to check. The values are set in days. Current options include: `1d`, `2d`, `3d`, and `4d`. |
| `value_type` | The kind of element. If kind is `NUMERIC`, the field is `value_type` and you can have up to four elements per card. If kind is `TIME_SERIES` or `BREAKDOWN`, the field is `value_types`. The maximum number of both `TIME_SERIES` or `BREAKDOWN` is 1. If you have numeric entries only, you can have up to four elements per card. If you want to use a combination, you can have up to two numeric entries and one of either time series or breakdown. You cannot have both time series and breakdown in the same card. If you define your value types as an array for time series, you can have up to three entries.  |
| `value_type`: `kind` | The type of value. Options include: `KRI` and `FINDING_COUNT`. |
| `value_type`: `finding_note_names` | If `kind` is `FINDING_COUNT`, the name of the findings that you want to see in your card, which is specified as an array. |
| `value_type`: `kri_note_names` | If `kind` is `FINDING_COUNT`, the name of the findings that you want to see in your card, which is specified as an array. |
| `value_type`: `text` | The text of the element type. The maximum number of characters is 22. |
{: class="simple-tab-table"}
{: caption="Table 4. Understanding the command's element components" caption-side="top"}
{: #definecard4}
{: tab-title="Elements"}
{: tab-group="card"}

Example response:

```
{
"author": {
  "account_id": "account id",
  "email": "email id",
  "id": "user id",
  "kind": "user"
},
"card": {
  "elements": [
    {
      "default_time_range": "1d",
      "kind": "NUMERIC",
      "text": "Count of findings reported by my security tool",
      "value_type": {
        "finding_note_names": [
          "providers/my-custom-tool/notes/my-custom-tool-findings-type"
        ],
        "kind": "FINDING_COUNT"
      }
    }
  ],
  "finding_note_names": [
    "providers/my-custom-tool/notes/my-custom-tool-findings-type"
  ],
  "section": "My Security Tools",
  "order": "1",
  "title": "My Security Tool Findings",
  "subtitle": "My Security Tool",
},
"context": {
  "account_id": "<account id>"
},
"create_time": "2018-09-04T11:49:36.056047Z",
"create_timestamp": 1536061776056,
"id": "custom-tool-card",
"kind": "CARD",
"long_description": "Details about why this is security risk to be fixed",
"name": "<account id>/providers/my-custom-tool/notes/custom-tool-card",
"provider_id": "my-custom-tool",
"provider_name": "<account id>/providers/my-custom-tool",
"reported_by": {
  "id": "my-custom-tool",
  "title": "My Security Tool"
},
"shared": true,
"short_description": "security risk found by my custom tool",
"update_time": "2018-09-04T11:49:36.056066Z",
"update_timestamp": 1536061776056,
"update_week_date": "2018-W36-2"
}
```
{: screen}


## Example usage
{: #custom-example}

Say that you have an application that runs on a {{site.data.keyword.containershort_notm}} cluster with the name `cloudkingdom`. Depending on the size of your application, you might have several pods within your cluster that you need to monitor all at the same time. What if you have multiple custom tools that monitor and detect your cluster for different threats. If one of your pods in the cluster starts to send an abnormal amount of data to external servers, you'd want to know as soon as possible. The custom tool that monitors data transfer can detect the finding and send it to {{site.data.keyword.security-advisor_short}}. If you have another custom integration that detects an issue, it too would send the finding to {{site.data.keyword.security-advisor_short}}. Then, {{site.data.keyword.security-advisor_short}} displays the findings from all of your monitoring tools in a single dashboard. There you can quickly see an overview of any alerts, investigate an issue, and learn about how to take remediation steps.

Example payload:

```
{
	"note_name": "<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
	"kind": "FINDING",
	"remediation": "How to resolve Data leakage threat",
	"provider_id": "my-custom-tool",
	"id": "my-custom-tool-finding-2",
	"context": {
		"region": "location",
		"resource_id": "cluster crn",
		"resource_name": "cloudkingdom",
		"resource_type": "container",
		"service_name": "kubernetes service"
	},
	"finding": {
		"severity": "HIGH",
		"next_steps": [{
			"title": "Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities",
                        "url":"https://cloud.ibm.com/containers-kubernetes/clusters"
		}],
                "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
                "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod’s normal behavior"
	}
}
```
{: screen}
