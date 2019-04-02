---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-13"

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


# 定制
{: #setup_custom}

通过 {{site.data.keyword.security-advisor_short}}，您可以集成现有的定制安全工具，无论是开放式源代码工具、定制开发的工具还是第三方服务均可。您可以通过将相应 URL 添加到集成列表来创建从 {{site.data.keyword.security-advisor_short}} 到其他工具的直接连接。此外，还可以使用 API 来配置 API 和关键风险指标 (KRI) 以集成第三方发现结果，从而在仪表板中的新卡上直接提供关键安全事件。
{: shortdesc}


## 添加直接连接
{: #setup-custom-gui}

可以使用 {{site.data.keyword.security-advisor_short}} 仪表板来轻松跟踪安全工具。
{: shortdesc}

### 开始之前
{: #custom-before-gui}

要能够添加集成，您必须首先具有要集成的合作伙伴的帐户。

{{site.data.keyword.security-advisor_short}} 不会持久存储与合作伙伴服务相关的任何凭证。企业用户必须使用 SAML 向 {{site.data.keyword.Bluemix_notm}} 和业务合作伙伴进行认证。
{: note}

### 配置连接
{: #custom-configure-connection}

1. 登录到安全工具并获取唯一 URL。

2. 使用控制台登录到 {{site.data.keyword.cloud_notm}}。

3. 单击**定制集成 > 直接连接**。这将显示一个屏幕。

  1. 为解决方案提供名称。只能在名称中使用字母数字字符、空格和短划线 (-)。

  2. 输入解决方案的 URL，格式为：`www.<website>.<domain>`。

  3. 上传用于表示工具的图标或图像。

  4. 单击**连接**以完成配置。{{site.data.keyword.security-advisor_short}} 将创建集成所需的工件，例如服务标识、API 密钥、帐户标识和元数据。分配的角色是 `writer`。



## 集成第三方发现结果

这些 API 遵循类 Grafeas 的工件元数据规范来存储、查询和检索安全工具和服务所报告的发现结果的关键元数据。
{: #setup-custom-api}

### 开始之前
{: #custom-before-api}

在集成来自第三方工具的发现结果之前，请确保您满足以下先决条件。

1. 确保使用的用户或服务标识分配有 [IAM 角色](https://cloud.ibm.com/iam#/users)**管理者**。

1. 登录到 {{site.data.keyword.cloud_notm}}。

  ```
  ibmcloud login
  ```
  {: codeblock}

2. 获取帐户标识。有关服务角色的更多信息，请查看 [{{site.data.keyword.security-advisor_short}} 访问策略](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)。

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

3. 获取 Identity and Access Management (IAM) 令牌。该令牌在每个 API 请求的 `--header` 中使用。

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  IAM 令牌每 60 分钟会到期一次。请了解如何使用 API 密钥来[自动获取新令牌](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey)。
  {: tip}



### 导入发现结果和 KRI
{: #custom-adding}

1. 通过创建注释来注册新的发现结果类型。要创建注释，请使用[发现结果 API](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote)。确保选择唯一的提供者标识来标识定制工具。如果要使用服务标识来自动执行流程，那么可以使用服务标识作为提供者标识。

  示例请求：

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Content-Type: application/json" -d "{ \"kind\": \"FINDING\", \"short_description\": \"My security tool finding\", \"long_description\": \"See what my custom security tool found\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-findings-type\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Custom Security Tool\" }, \"finding\": { \"severity\": \"MEDIUM\", \"next_steps\": [ { \"title\": \"Learn why this is reported as a risk\" } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="“更多信息”图标"/> 了解此命令的组成部分</th>
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
        <td>解决此问题需要采取的步骤。</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>定制安全工具。</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>安全工具找到的发现结果类型的标识。</td>
      </tr>
      <tr>
        <td><code>context</code><ul><li><code>region</code></li><li><code>resource_id</code></li> <li><code>resource_name</code></li> <li><code>resource_type</code></li> <li><code>service_name</code></li></ul></td>
        <td></br><ul><li><code>出现发现结果的位置。</code></li><li><code>特定资源的标识。</code></li> <li><code>资源的名称。</code></li> <li><code>资源的类型。</code></li> <li><code>服务的名称。</code></li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>url</code></li></ul></td>
        <td></br><ul><li>发现结果显示的紧急程度级别。</li> <li>为修复此问题可以采取的步骤。</li> <li>可以在其中找到发现结果详细信息的 URL。</li></ul></td>
      </tr>
    </tbody>
  </table>

  示例响应：

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

  确保记住作为响应一部分返回的注释的名称。在此示例中，值为 `/providers/my-custom-tool/notes/my-custom-tool-findings-type`。下一步中将使用此值。
  {: tip}

2. 将发现结果发布为 KRI 或事件，也称为[事件实例](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence)。

  对于每个卡，可以定义两个 KRI。
  {: note}

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account-id>/providers/my-custom-tool/occurrences" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Replace-If-Exists: true" -H "Content-Type: application/json" -d "{ \"note_name\": \"<account-id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\", \"kind\": \"FINDING\", \"remediation\": \"how to resolve this\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-finding-1\", \"context\": { \"region\": \"location\", \"resource_id\": \"pluginId\", \"resource_name\": \"www.myapp.com\", \"resource_type\": \"worker\", \"service_name\": \"application\" }, \"finding\": { \"severity\": \"HIGH\", \"next_steps\": [{ \"url\": \"Details URL\" }] }}"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="“更多信息”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>概述发现结果的简短描述；至多几个字。</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>包含有关发现结果的更多详细信息的更详细描述。</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>定制安全工具。</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>安全工具找到的发现结果类型的标识。</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>报告发现结果的安全工具的标识。</li><li>报告发现结果的安全工具的标题。</li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>title</code></li></ul></td>
        <td></br><ul><li>发现结果显示的紧急程度级别。</li> <li>为修复此问题可以采取的步骤。</li> <li>发现结果的标题。</li></ul></td>
      </tr>
    </tbody>
  </table>

  示例响应：

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

3. 在仪表板中定义卡，以通过创建[注释](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote)来显示发现结果。

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam token>" -H "Content-Type: application/json" -d "{ \"kind\": \"CARD\", \"provider_id\": \"my-custom-tool\", \"id\": \"custom-tool-card\", \"short_description\": \"security risk found by my custom tool\", \"long_description\": \"Details about why this is security risk to be fixed\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Security Tool\" }, \"card\": { \"section\": \"My Security Tools\", \"title\": \"My Security Tool Findings\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ], \"elements\": [ { \"kind\": \"NUMERIC\", \"text\": \"Count of findings reported by my security tool\", \"default_time_range\": \"1d\", \"value_type\": { \"kind\": \"FINDING_COUNT\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ] } } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="“更多信息”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>CARD</code></td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>定制安全工具。</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>安全工具找到的发现结果类型的标识。</td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>概述发现结果的描述，至多几个字。</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>包含有关发现结果的更多详细信息的更详细描述。</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>报告发现结果的安全工具的标识。</li><li>报告发现结果的安全工具的标题。</li></ul></td>
      </tr>
      <tr>
        <td><code>card</code> <ul><li><code>section</code></li> <li><code>title</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>卡适合放入其中的部分。</li> <li>卡的标题</li> <li><code>providers/<provider_id>/notes/my-custom-tool-findings-type</code></li></ul></td>
      </tr>
      <tr>
        <td><code>elements</code><ul><li><code>kind</code></li> <li><code>text</code></li> <li><code>default_time_range</code></li></ul></td>
        <td></br><ul><li>元素的类型。</li> <li>要显示的文本。</li> <li>要检查的时间量。</li></ul></td>
      </tr>
      <tr>
        <td><code>value_type</code><ul><li><code>kind</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>值的类型。</li> <li>要在卡中查看的发现结果的名称。</li></ul></td>
      </tr>
    </tbody>
  </table>

  示例响应：
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

4. 浏览至服务仪表板以查看创建的卡。

## 示例用法
{: #custom-example}

假设您有一个在 {{site.data.keyword.containershort_notm}} 集群上运行的应用程序，名称为 `cloudkingdom`。根据应用程序的大小，集群中可能有多个 pod 要同时进行监视。如果您有多个定制工具用于监视和检测集群中的不同威胁，该怎么办呢？如果集群中的其中一个 pod 开始向外部服务器发送异常数量的数据，那么您会希望尽快知道这一情况。用于监视数据传输的定制工具可以检测发现结果，并将其发送到 {{site.data.keyword.security-advisor_short}}。如果您有另一个定制集成用于检测问题，那么该集成也会将发现结果发送到 {{site.data.keyword.security-advisor_short}}。然后，{{site.data.keyword.security-advisor_short}} 会在单个仪表板中显示来自所有监视工具的发现结果。您可以在其中快速查看任何警报的概述，调查问题并了解如何采取修复步骤。


示例有效内容：

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
