---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-06"

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


# 定制发现结果
{: #setup_custom}

通过 {{site.data.keyword.security-advisor_short}}，您可以集成现有的定制安全工具，无论是开放式源代码、定制开发还是第三方服务均可。通过集成第三方发现结果，可以将所有安全工具和发现结果合并到一个仪表板中，在其中可以监视关键安全事件。
{: shortdesc}


## 开始之前
{: #custom-before-api}

在集成来自第三方工具的发现结果之前，请确保您满足以下先决条件。

1. 确保使用的用户或服务标识分配有 [IAM 角色](https://cloud.ibm.com/iam#/users)**管理者**。

2. 登录到 {{site.data.keyword.cloud_notm}}。

  ```
  ibmcloud login
  ```
  {: codeblock}

3. 获取帐户标识。有关服务角色的更多信息，请查看 [{{site.data.keyword.security-advisor_short}} 访问策略](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)。

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

4. 获取 Identity and Access Management (IAM) 令牌。该令牌在每个 API 请求的 `--header` 中使用。

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  IAM 令牌每 60 分钟会到期一次。请了解如何使用 API 密钥来[自动获取新令牌](/docs/iam?topic=iam-iamtoken_from_apikey)。
  {: tip}



## 导入发现结果和 KRI
{: #custom-adding}

这些 API 遵循类 Grafeas 的工件元数据规范来存储、查询和检索安全工具和服务所报告的发现结果的关键元数据。
可以通过使用 API 配置关键风险指标 (KRI) 来完成集成。
{: shortdesc}


### 步骤 1：注册新的发现结果类型
{: #custom-register-finding}

要注册新类型的发现结果，您可以创建注释。要创建注释，可以使用[发现结果 API](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}。确保选择唯一的提供者标识来标识定制工具。如果要自动执行流程，可以使用服务标识作为提供者标识。

示例请求：

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{  \"kind\": \"FINDING\",  \"short_description\": \"My security tool finding\",  \"long_description\": \"Longer description of what the security tool found.\",  \"provder_id\": \"my-custom-tool\",  \"id\": \"my-custom-tool-findings-type\",  \"reported_by\": {    \"id\": \"my-custom-tool\",    \"title\": \"My custom security tool\"  } ,  \"finding\": {    \"severity\": \"MEDIUM\",    \"next_steps\": [      {        \"title\": \"Explain why it's reported as a risk.\"      }    ]  }}"
```
{: codeblock}

|常规|描述| 
|:-----------------|:-----------------|
|`note_name`|`<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type`|
|`kind`|`FINDING`|
|`remediation`|解决此问题需要采取的步骤。|
|`provider_id`|定制安全工具。|
|`id`|安全工具找到的发现结果类型的标识。|
{: class="simple-tab-table"}
{: caption="表 1. 了解命令的常规组成部分" caption-side="top"}
{: #registerfindingtable1}
{: tab-title="General"}
{: tab-group="register"}

|上下文|描述| 
|:-----------------|:-----------------|
|`region`|出现发现结果的位置。|
|`resource_id`|资源的标识。|
|`resource_name`|资源的名称。|
|`resource_type`|资源的类型。|
|`service_name`|服务的名称。|
{: class="simple-tab-table"}
{: caption="表 2. 了解命令的上下文组成部分" caption-side="top"}
{: #registerfinding2}
{: tab-title="Context"}
{: tab-group="register"}

|发现结果|描述| 
|:-----------------|:-----------------|
|`severity`|发现结果显示的紧急程度级别。|
|`next_steps`|为修复此问题可以采取的步骤。|
|`url`|可以在其中找到发现结果详细信息的 URL。|
{: class="simple-tab-table"}
{: caption="表 3. 了解命令的发现结果组成部分" caption-side="top"}
{: #registerfinding3}
{: tab-title="Finding"}
{: tab-group="register"}

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


### 步骤 2：发布发现结果
{: #custom-post-findings}

创建[事件实例](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence){: external}，以将发现结果作为 KRI 或事件发布到安全性顾问程序仪表板。

对于每个卡，可以定义两个 KRI。
{: note}

示例请求： 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/occurrences"  -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"note_name\": \"<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\",    \"kind\": \"FINDING\",    \"remediation\": \"How to resolve Data leakage threat\",    \"provider_id\": \"my-custom-tool\",    \"id\": \"my-custom-tool-finding-2\",    \"context\": {        \"region\": \"location\",        \"resource_id\": \"cluster crn\",        \"resource_name\": \"cloudkingdom\",        \"resource_type\": \"container\",        \"service_name\": \"kubernetes service\"    },    \"finding\": {        \"severity\": \"HIGH\",        \"next_steps\": [{            \"title\": \"Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities\",                        \"url\":\"https://cloud.ibm.com/containers-kubernetes/clusters\"        }],                \"short_description\": \"One of the pods in your cluster appears to be leaking an excessive amount of data\",                \"long_description\": \"One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod's normal behavior\"    }}"
```
{: codeblock}

|常规|描述| 
|:-----------------|:-----------------|
|`kind`|`FINDING`|
|`short_description`|概述发现结果的简短描述；至多几个字。|
|`long_description`|包含有关发现结果的更多详细信息的更详细描述。|
|`provider_id`|定制安全工具。|
|`id`|安全工具找到的发现结果类型的标识。|
{: class="simple-tab-table"}
{: caption="表 2. 了解命令的常规组成部分" caption-side="top"}
{: #postfinding1}
{: tab-title="General"}
{: tab-group="post"}

|报告者|描述| 
|:-----------------|:-----------------|
|`id`|报告发现结果的安全工具的标识。|
|`title`|报告发现结果的安全工具的标题。|
{: class="simple-tab-table"}
{: caption="表 2. 了解命令的报告者组成部分" caption-side="top"}
{: #postfinding2}
{: tab-title="Reported by"}
{: tab-group="post"}

|发现结果|描述| 
|:-----------------|:-----------------|
|`severity`|发现结果显示的紧急程度级别。|
|`next_steps`|为修复此问题可以采取的步骤。|
|`title`|发现结果的标题。|
{: class="simple-tab-table"}
{: caption="表 3. 了解命令的发现结果组成部分" caption-side="top"}
{: #postfinding3}
{: tab-title="Finding"}
{: tab-group="post"}


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

### 步骤 3：定义要显示的卡
{: #custom-define-card}

通过创建[注释](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}，定义希望卡如何在仪表板中显示发现结果。

示例请求： 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"kind\": \"CARD\",        \"provider_id\": \"my-custom-tool\",    \"id\": \"custom-tool-card\",    \"short_description\": \"Security risk found by my custom tool\",        \"long_description\": \"More detailed description about why this security risk needs to be fixed\",    \"reported_by\": {      \"id\": \"my-custom-tool\",      \"title\": \"My security tool\"    },        \"card\": {      \"section\": \"My security tools\",      \"order\": 1 ,     \"title\": \"My security tool findings\",      \"subtitle\": \"My security tool\",      \"finding_note_names\": [        \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"      ],      \"elements\": [        {          \"kind\": \"NUMERIC\",          \"text\": \"Count of findings reported by my security tool\",          \"default_time_range\": \"1d\",          \"value_type\": {            \"kind\": \"FINDING_COUNT\",            \"finding_note_names\": [              \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"            ]          }        }      ]    }  }"
```
{: codeblock}

|常规|描述| 
|:-----------------|:-----------------|
|`provider_id`|安全工具的标识。|
|`id`|安全工具找到的发现结果类型的标识。|
|`short_description`|概述发现结果的简短描述；至多几个字。|
|`long_description`|包含有关发现结果的更多详细信息的更详细描述。|
{: class="simple-tab-table"}
{: caption="表 1. 了解命令的常规组成部分" caption-side="top"}
{: #definecard1}
{: tab-title="General"}
{: tab-group="card"}

|报告者|描述| 
|:-----------------|:-----------------|
|`id`|报告发现结果的安全工具的标识。|
|`title`|报告发现结果的安全工具的标题。|
{: class="simple-tab-table"}
{: caption="表 2. 了解命令的报告者组成部分" caption-side="top"}
{: #definecard2}
{: tab-title="Reported by"}
{: tab-group="card"}

|卡|描述| 
|:-----------------|:-----------------|
|`section`|希望卡在其中显示的部分。最多可以有三个定制部分，每个部分可有六张卡。最大字符数：30|
|可选：`order`|卡在指定部分中的显示顺序。指定的顺序的范围为 1 - 6。如果选择的数字已应用于其他卡，那么创建操作会失败。您会收到一条错误消息，声明“给定顺序已由部分中的其他卡使用”。如果提供的顺序大于当前卡数加 1，那么卡创建会失败。例如，如果您当前有两张卡，并且要创建另一张卡，那么不能在卡顺序中指定 5，因为总共只有三张卡。如果未指定卡的顺序，那么卡会按字母顺序在指定的部分中排列。|
|`title`|希望卡具有的标题。最大字符数：28|
|`subtitle`|希望卡具有的子标题。最大字符数：30|
|`finding_note_names`|`providers//notes/my-custom-tool-findings-type`|
{: class="simple-tab-table"}
{: caption="表 3. 了解命令的卡组成部分" caption-side="top"}
{: #definecard3}
{: tab-title="Card"}
{: tab-group="card"}

|元素：值类型|描述| 
|:-----------------|:-----------------|
|`kind`|选项包括：`NUMERIC`、`TIME_SERIES` 和 `BREAKDOWN`。|
|`text`|要显示的文本。如果 kind 为 `NUMERIC`，那么最大字符数为 60。如果 kind 为 `TIME_SERIES` 或 `BREAKDOWN`，那么最大字符数为 65。|
|`default_time_range`|要检查的时间量。设置的值以天为单位。当前选项包括：`1d`、`2d`、`3d` 和 `4d`。|
|`value_type`|元素的类型。如果 kind 为 `NUMERIC`，那么此字段为 `value_type`，并且每张卡最多可以有四个元素。如果 kind 为 `TIME_SERIES` 或 `BREAKDOWN`，那么此字段为 `value_types`。`TIME_SERIES` 或 `BREAKDOWN` 的最大值均为 1。如果您只有数字条目，那么每张卡最多可以有四个元素。如果要使用组合，那么最多可以包含两个数字条目，以及时间序列或细分条目中的任一个条目。不能在同一张卡中同时包含时间序列和细分。如果将值类型定义为时间系列的数组，那么最多可以有三个条目。|
|`value_type`: `kind`|值的类型。选项包括：`KRI` 和 `FINDING_COUNT`。|
|`value_type`: `finding_note_names`|如果 `kind` 为 `FINDING_COUNT`，那么此项为要在卡中查看的发现结果的名称（指定为数组）。|
|`value_type`: `kri_note_names`|如果 `kind` 为 `FINDING_COUNT`，那么此项为要在卡中查看的发现结果的名称（指定为数组）。|
|`value_type`: `text`|元素类型的文本。最大字符数为 22。|
{: class="simple-tab-table"}
{: caption="表 4. 了解命令的元素组成部分" caption-side="top"}
{: #definecard4}
{: tab-title="Elements"}
{: tab-group="card"}

示例响应：

```
{
"author": {
      "account_id": "account id",
      "email": "email id ",
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


## 示例用法
{: #custom-example}

假设您有一个在 {{site.data.keyword.containershort_notm}} 集群上运行的应用程序，名称为 `cloudkingdom`。根据应用程序的大小，集群中可能有多个 pod 需要同时进行监视。如果您有多个定制工具用于监视和检测集群中的不同威胁，该怎么办呢？如果集群中的其中一个 pod 开始向外部服务器发送异常数量的数据，那么您会希望尽快知道这一情况。用于监视数据传输的定制工具可以检测发现结果，并将其发送到 {{site.data.keyword.security-advisor_short}}。如果您有另一个定制集成用于检测问题，那么该集成也会将发现结果发送到 {{site.data.keyword.security-advisor_short}}。然后，{{site.data.keyword.security-advisor_short}} 会在单个仪表板中显示来自所有监视工具的发现结果。您可以在其中快速查看任何警报的概述，调查问题并了解如何采取修复步骤。

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
                        "url":"https://cloud.ibm.com/containers-kubernetes/clusters"
		}],
                "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
                "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod’s normal behavior"
	}
}
```
{: screen}
