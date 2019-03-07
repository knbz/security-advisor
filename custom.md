---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-07"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

---

{:new_window: target="_blank"}
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


# Custom
{: #setup_custom}

{{site.data.keyword.security-advisor_short}} enables you to integrate your existing custom security tools, whether open source, custom developed, or 3rd party services. You can create a direct connection from {{site.data.keyword.security-advisor_short}} to the other tool by adding the URL to your list of integrations. You can also integrate 3rd party findings to bring critical security events directly into a new card in the dashboard by using the APIs to configure APIs and key risk indicators (KRIs).
{: shortdesc}


## Adding a direct connection
{: #setup-custom-gui}

You can easily track your security tools by using the {{site.data.keyword.security-advisor_short}} dashboard.
{: shortdesc}

### Before you begin
{: #custom-before-gui}

Before you can add the integration, you must first have an account with the partner that you want to integrate.

{{site.data.keyword.security-advisor_short}} does not persist any credentials that are related to the partner service. Enterprise users must authenticate by using SAML to both {{site.data.keyword.Bluemix_notm}} and to the business partner.
{: note}

### Configuring the connection
{: #custom-configure-connection}

1. Log in to your security tool and get your unique URL.

2. Log in to {{site.data.keyword.cloud_notm}} by using the console.

3. Click **Custom Integrations > Direct Connection**. A screen displays.

  1. Give your solution a name. You can use only alpha-numeric characters, white spaces, and dashes (-) in the name.

  2. Enter the URL for the solution in the format: `www.<website>.<domain>`.

  3. Upload an icon or image to represent the tool.

  4. Click **Connect** to complete the configuration. {{site.data.keyword.security-advisor_short}} creates the artifacts that are required for integration such as the service ID, API key, account ID, and metadata. The `writer` role is assigned.



## Integrate 3rd party findings

The APIs follow Grafeas like artifact metadata specifications to store, query, and retrieve the critical metadata for the findings that are reported by your security tools and services.
{: #setup-custom-api}

### Before you begin
{: #custom-before-api}

Before you integrate findings from your 3rd party tool, be sure that you have the following prerequisites.

1. Be sure that the user or service ID that you're using is assigned the **Manager** [IAM role](https://console.bluemix.net/iam/#/users).

1. Log in to {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud login
  ```
  {: codeblock}

2. Get your account ID. For more information about service roles, check out the [{{site.data.keyword.security-advisor_short}} access policies](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

3. Get your Identity and Access Management (IAM) token. The token is used in the `--header` of each API request.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  IAM tokens expire every 60 minutes. To learn how to [get a new token automatically](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey) by using an API key.
  {: tip}

</br>

### Importing findings and KRIs
{: #custom-adding}

1. Register a new type of Finding by creating a note. To create the note, use the [Findings API](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote). Be sure that you choose a unique provider ID to identify your custom tool. If you're automating the process by using a service ID you can use the service ID as your provider ID.

  Example request:

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Content-Type: application/json" -d "{ \"kind\": \"FINDING\", \"short_description\": \"My security tool finding\", \"long_description\": \"See what my custom security tool found\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-findings-type\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Custom Security Tool\" }, \"finding\": { \"severity\": \"MEDIUM\", \"next_steps\": [ { \"title\": \"Learn why this is reported as a risk\" } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="More information icon"/> Understanding this commands components </th>
    </thead>
    <tbody>
      <tr>
        <td><code>note_name</code></td>
        <td><code>&lt;account id&gt;/providers/my-custom-tool/notes/my-custom-tool-findings-type</code></td>
      </tr>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>remediation</code></td>
        <td>The steps that need to be taken to resolve the issue.</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>Your custom security tool.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>An ID for the type of finding that your security tool found.</td>
      </tr>
      <tr>
        <td><code>context</code><ul><li><code>region</code></li><li><code>resource_id</code></li> <li><code>resource_name</code></li> <li><code>resource_type</code></li> <li><code>service_name</code></li></ul></td>
        <td></br><ul><li><code>The location in which the finding occurred</code></li><li><code>The ID for the specific resource.</code></li> <li><code>The name of the resource.</code></li> <li><code>The type of resource.</code></li> <li><code>The name of the service.</code></li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>url</code></li></ul></td>
        <td></br><ul><li>The level of urgency that the finding presents.</li> <li>The steps that can be taken to remediate the issue.</li> <li>A URL where the details of the finding can be found.</li></ul></td>
      </tr>
    </tbody>
  </table>

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

2. Post findings as KRIs or events, otherwise known as an [occurrence](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence).

  For each card, you can define two KRIs.
  {: note}

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account-id>/providers/my-custom-tool/occurrences" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Replace-If-Exists: true" -H "Content-Type: application/json" -d "{ \"note_name\": \"<account-id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\", \"kind\": \"FINDING\", \"remediation\": \"how to resolve this\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-finding-1\", \"context\": { \"region\": \"location\", \"resource_id\": \"pluginId\", \"resource_name\": \"www.myapp.com\", \"resource_type\": \"worker\", \"service_name\": \"application\" }, \"finding\": { \"severity\": \"HIGH\", \"next_steps\": [{ \"url\": \"Details URL\" }] }}"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="More information icon"/> Understanding this commands components </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>A short description that summarizes the finding; no more than a couple of words.</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>A longer description that contains more detail about the finding.</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>Your custom security tool.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>An ID for the type of finding that your security tool found.</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>The ID of the security tool that reported the finding.</li><li>The title of the security tool that reported the finding.</li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>title</code></li></ul></td>
        <td></br><ul><li>The level of urgency that the finding presents.</li> <li>The steps that can be taken to remediate the issue.</li> <li>The title of the finding.</li></ul></td>
      </tr>
    </tbody>
  </table>

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

3. Define the card in the dashboard to display your finding by creating a [note](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote).

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam token>" -H "Content-Type: application/json" -d "{ \"kind\": \"CARD\", \"provider_id\": \"my-custom-tool\", \"id\": \"custom-tool-card\", \"short_description\": \"security risk found by my custom tool\", \"long_description\": \"Details about why this is security risk to be fixed\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Security Tool\" }, \"card\": { \"section\": \"My Security Tools\", \"title\": \"My Security Tool Findings\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ], \"elements\": [ { \"kind\": \"NUMERIC\", \"text\": \"Count of findings reported by my security tool\", \"default_time_range\": \"1d\", \"value_type\": { \"kind\": \"FINDING_COUNT\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ] } } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="More information icon"/> Understanding this commands components </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>CARD</code></td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>Your custom security tool.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>An ID for the type of finding that your security tool found.</td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>A description, no more than a couple words, that summarizes the finding.</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>A longer description that contains more detail about the finding.</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>The ID of the security tool that reported the finding.</li><li>The title of the security tool that reported the finding.</li></ul></td>
      </tr>
      <tr>
        <td><code>card</code> <ul><li><code>section</code></li> <li><code>title</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>The section that the card fits into.</li> <li>The title of the card</li> <li><code>providers/<provider_id>/notes/my-custom-tool-findings-type</code></li></ul></td>
      </tr>
      <tr>
        <td><code>elements</code> <ul><li><code>kind</code></li> <li><code>text</code></li> <li><code>default_time_range</code></li></ul></td>
        <td></br><ul><li>The type of element.</li> <li>The text that you want to display</li> <li>The amount of time that you want to check.</li></ul></td>
      </tr>
      <tr>
        <td><code>value_type</code> <ul><li><code>kind</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>The type of value</li> <li>The name of the findings that you want to see in your card.</li></ul></td>
      </tr>
    </tbody>
  </table>

  Example response:
  ```
    {
    "author": {
      "account_id": "<account id",
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
      "title": "My Security Tool Findings"
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

4. Navigate to your service dashboard to see the card that you created.

## Example usage
{: #custom-example}

Say that you have an application that runs on a {{site.data.keyword.containershort_notm}} cluster with the name `cloudkingdom`. Depending on the size of your application, you might have several pods within your cluster to monitor all at the same time. What if you have multiple custom tools that monitor and detect your cluster for different threats. If one of your pods in the cluster starts to send an abnormal amount of data to external servers, you'd want to know as soon as possible. The custom tool that monitors data transfer can detect the finding and send it to {{site.data.keyword.security-advisor_short}}. If you have another custom integration that detects an issue, it too would send the finding to {{site.data.keyword.security-advisor_short}}. Then, {{site.data.keyword.security-advisor_short}} displays the findings from all of your monitoring tools in a single dashboard. There you can quickly see an overview of any alerts, investigate an issue, and learn about how to take remediation steps.


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
                        "url":"https://console.bluemix.net/containers-kubernetes/clusters"
		}],
                "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
                "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod’s normal behavior"
	}
}
```
{: screen}
