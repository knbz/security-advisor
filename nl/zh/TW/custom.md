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


# 自訂發現項目
{: #setup_custom}

使用 {{site.data.keyword.security-advisor_short}}，您可以整合現有的自訂安全工具，不論是開放程式碼、自訂開發還是協力廠商服務均可。藉由整合協力廠商發現項目，可以將所有安全工具和發現項目合併到一個儀表板中，您可以在其中監視關鍵安全事件。
{: shortdesc}


## 開始之前
{: #custom-before-api}

整合來自協力廠商工具的發現項目之前，請確定您具有下列必要條件。

1. 確定您使用的使用者或服務 ID 已獲指派**管理員** [IAM 角色](https://cloud.ibm.com/iam#/users)。

2. 登入 {{site.data.keyword.cloud_notm}}。

  ```
  ibmcloud login
  ```
  {: codeblock}

3. 取得帳戶 ID。如需服務角色的相關資訊，請參閱 [{{site.data.keyword.security-advisor_short}}存取原則](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)。

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

4. 取得 Identity and Access Management (IAM) 記號。記號用於每個 API 要求的 `--header` 中。

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  IAM 記號會每 60 分鐘到期一次。瞭解如何使用 API 金鑰來[自動取得新的記號](/docs/iam?topic=iam-iamtoken_from_apikey)。
  {: tip}



## 匯入發現項目及 KRI
{: #custom-adding}

這些 API 遵循類似 Grafeas 的構件 meta 資料規格，來儲存、查詢與擷取安全工具及服務所報告之發現項目的重要 meta 資料。可以藉由使用 API 配置關鍵風險指標 (KRI) 來完成整合。
{: shortdesc}


### 步驟 1：登錄新的發現項目類型
{: #custom-register-finding}

若要登錄新類型的發現項目，您可以建立附註。若要建立附註，可以使用[發現項目 API](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}。請確定您選擇唯一的提供者 ID 來識別自訂工具。如果要自動化流程，可以使用服務 ID 作為提供者 ID。

範例要求：

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{  \"kind\": \"FINDING\",  \"short_description\": \"My security tool finding\",  \"long_description\": \"Longer description of what the security tool found.\",  \"provder_id\": \"my-custom-tool\",  \"id\": \"my-custom-tool-findings-type\",  \"reported_by\": {    \"id\": \"my-custom-tool\",    \"title\": \"My custom security tool\"  } ,  \"finding\": {    \"severity\": \"MEDIUM\",    \"next_steps\": [      {        \"title\": \"Explain why it's reported as a risk.\"      }    ]  }}"
```
{: codeblock}

| 一般 |說明| 
|:-----------------|:-----------------|
| `note_name` | `<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type`  |
| `kind` | `FINDING` |
| `remediation` |解決問題所需採取的步驟。|
| `provider_id` |自訂安全工具。|
| `id` |安全工具所找到的發現項目類型 ID。|
{: class="simple-tab-table"}
{: caption="表 1. 瞭解指令一般元件" caption-side="top"}
{: #registerfindingtable1}
{: tab-title="General"}
{: tab-group="register"}

|環境定義|說明| 
|:-----------------|:-----------------|
| `region` |出現發現項目的位置。|
| `resource_id` |資源的 ID。|
| `resource_name` |資源的名稱。|
| `resource_type` |資源的類型。|
| `service_name` | 服務的名稱。|
{: class="simple-tab-table"}
{: caption="表 2. 瞭解指令環境定義元件" caption-side="top"}
{: #registerfinding2}
{: tab-title="Context"}
{: tab-group="register"}

|發現項目|說明| 
|:-----------------|:-----------------|
| `severity` |發現項目所呈現的緊急程度。|
| `next_steps` |重新修補問題時可以採取的步驟。|
| `url` |可以找到發現項目詳細資料的 URL。|
{: class="simple-tab-table"}
{: caption="表 3. 瞭解指令發現項目元件" caption-side="top"}
{: #registerfinding3}
{: tab-title="Finding"}
{: tab-group="register"}

範例回應：

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

請務必記住作為回應一部分傳回的附註名稱。在此範例中，值是 `/providers/my-custom-tool/notes/my-custom-tool-findings-type`。下一步中會使用此值。
{: tip}


### 步驟 2：公佈發現項目
{: #custom-post-findings}

建立[事件實例](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence){: external}，以將發現項目作為 KRI 或事件公佈到 Security Advisor 儀表板。

您可以針對每張卡片，定義兩個 KRI。
{: note}

範例要求： 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/occurrences"  -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"note_name\": \"<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\",    \"kind\": \"FINDING\",    \"remediation\": \"How to resolve Data leakage threat\",    \"provider_id\": \"my-custom-tool\",    \"id\": \"my-custom-tool-finding-2\",    \"context\": {        \"region\": \"location\",        \"resource_id\": \"cluster crn\",        \"resource_name\": \"cloudkingdom\",        \"resource_type\": \"container\",        \"service_name\": \"kubernetes service\"    },    \"finding\": {        \"severity\": \"HIGH\",        \"next_steps\": [{            \"title\": \"Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities\",                        \"url\":\"https://cloud.ibm.com/containers-kubernetes/clusters\"        }],                \"short_description\": \"One of the pods in your cluster appears to be leaking an excessive amount of data\",                \"long_description\": \"One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod's normal behavior\"    }}"
```
{: codeblock}

| 一般 |說明| 
|:-----------------|:-----------------|
| `kind` | `FINDING` |
| `short_description` |彙總發現項目的簡要說明；不得超過幾個單字。|
| `long_description` |含有更多有關發現項目之詳細資料的較詳細說明。|
| `provider_id` |自訂安全工具。|
| `id` |安全工具所找到的發現項目類型 ID。|
{: class="simple-tab-table"}
{: caption="表 2. 瞭解指令一般元件" caption-side="top"}
{: #postfinding1}
{: tab-title="General"}
{: tab-group="post"}

|報告者|說明| 
|:-----------------|:-----------------|
| `id` |已報告發現項目的安全工具 ID。|
| `title` |已報告發現項目的安全工具標題。|
{: class="simple-tab-table"}
{: caption="表 2. 瞭解指令報告者元件" caption-side="top"}
{: #postfinding2}
{: tab-title="Reported by"}
{: tab-group="post"}

|發現項目|說明| 
|:-----------------|:-----------------|
| `severity` |發現項目所呈現的緊急程度。|
| `next_steps` |重新修補問題時可以採取的步驟。|
| `title` |發現項目的標題。|
{: class="simple-tab-table"}
{: caption="表 3. 瞭解指令發現項目元件" caption-side="top"}
{: #postfinding3}
{: tab-title="Finding"}
{: tab-group="post"}


範例回應：

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

### 步驟 3：定義要顯示的卡片
{: #custom-define-card}

藉由建立[附註](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}，定義您要卡片如何在儀表板中顯示發現項目。

範例要求： 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"kind\": \"CARD\",        \"provider_id\": \"my-custom-tool\",    \"id\": \"custom-tool-card\",    \"short_description\": \"Security risk found by my custom tool\",        \"long_description\": \"More detailed description about why this security risk needs to be fixed\",    \"reported_by\": {      \"id\": \"my-custom-tool\",      \"title\": \"My security tool\"    },        \"card\": {      \"section\": \"My security tools\",      \"order\": 1 ,     \"title\": \"My security tool findings\",      \"subtitle\": \"My security tool\",      \"finding_note_names\": [        \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"      ],      \"elements\": [        {          \"kind\": \"NUMERIC\",          \"text\": \"Count of findings reported by my security tool\",          \"default_time_range\": \"1d\",          \"value_type\": {            \"kind\": \"FINDING_COUNT\",            \"finding_note_names\": [              \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"            ]          }        }      ]    }  }"
```
{: codeblock}

| 一般 |說明| 
|:-----------------|:-----------------|
| `provider_id` |安全工具的 ID。|
| `id` |安全工具所找到的發現項目類型 ID。|
| `short_description` |彙總發現項目的簡要說明；不得超過幾個單字。|
| `long_description` |含有更多有關發現項目之詳細資料的較詳細說明。|
{: class="simple-tab-table"}
{: caption="表 1. 瞭解指令一般元件" caption-side="top"}
{: #definecard1}
{: tab-title="General"}
{: tab-group="card"}

|報告者|說明| 
|:-----------------|:-----------------|
| `id` |已報告發現項目的安全工具 ID。|
| `title` |已報告發現項目的安全工具標題。|
{: class="simple-tab-table"}
{: caption="表 2. 瞭解指令報告者元件" caption-side="top"}
{: #definecard2}
{: tab-title="Reported by"}
{: tab-group="card"}

|卡片 |說明| 
|:-----------------|:-----------------|
|`section`|希望卡片在其中顯示的區段。最多可以有三個自訂區段，每個區段可有六張卡片。字元數上限：30  |
|選用：`order`|卡片在指定區段中的顯示順序。指定的順序範圍為 1 - 6。如果選擇的數字已套用於其他卡片，建立作業會失敗。您會收到一條錯誤訊息，指出「給定順序已由區段中的其他卡片使用」。如果提供的順序大於現行卡片數加 1，卡片建立作業會失敗。例如，如果您現行有兩張卡片，並且要建立另一張卡片，則不能在卡片順序中指定 5，因為總共只有三張卡片。如果未指定卡片的順序，卡片會按字母順序在指定的區段中排列。|
| `title` |希望卡片具有的標題。字元數上限：28  |
|`subtitle`|希望卡片具有的子標題。字元數上限：30  |
|`finding_note_names`|`providers//notes/my-custom-tool-findings-type`|
{: class="simple-tab-table"}
{: caption="表 3. 瞭解指令卡片元件" caption-side="top"}
{: #definecard3}
{: tab-title="Card"}
{: tab-group="card"}

|元素：值類型|說明| 
|:-----------------|:-----------------|
| `kind` |選項包括：`NUMERIC`、`TIME_SERIES` 和 `BREAKDOWN`。|
|`text`|要顯示的文字。如果 kind 為 `NUMERIC`，字元數上限為 60。如果 kind 為 `TIME_SERIES` 或 `BREAKDOWN`，字元數上限為 65。|
|`default_time_range`|您要檢查的時間量。設定的值以天為單位。現行選項包括：`1d`、`2d`、`3d` 和 `4d`。|
|`value_type`|元素的類型。如果 kind 為 `NUMERIC`，則此欄位為 `value_type`，並且每張卡片最多可以有四個元素。如果 kind 為 `TIME_SERIES` 或 `BREAKDOWN`，此欄位為 `value_types`。`TIME_SERIES` 或 `BREAKDOWN` 的最大均為 1。如果您只有數字項目，每張卡片最多可以有四個元素。如果要使用組合，最多可以包含兩個數字項目，以及時間序列或分解中的任一個項目。不能在同一張卡片中同時包含時間序列和分解。如果將值類型定義為時間序列的陣列，最多可以有三個項目。|
|`value_type`: `kind`|值的類型。選項包括：`KRI` 和 `FINDING_COUNT`。|
|`value_type`: `finding_note_names`|如果 `kind` 是 `FINDING_COUNT`，此為您要在卡片中看到的發現項目名稱，它是指定為陣列。|
|`value_type`: `kri_note_names`|如果 `kind` 是 `FINDING_COUNT`，此為您要在卡片中看到的發現項目名稱，它是指定為陣列。|
|`value_type`: `text`|元素類型的文字。上限字元數為 22。|
{: class="simple-tab-table"}
{: caption="表 4. 瞭解指令元素元件" caption-side="top"}
{: #definecard4}
{: tab-title="Elements"}
{: tab-group="card"}

範例回應：

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


## 範例用法
{: #custom-example}

假設，您的應用程式在具有名稱 `cloudkingdom` 的 {{site.data.keyword.containershort_notm}} 叢集上執行。根據應用程式的大小，叢集中可能有多個 pod 需要同時進行監視。如果您有多個自訂工具可監視並偵測叢集是否有不同的威脅，則要怎麼做。如果叢集中的其中一個 Pod 開始將異常數量的資料傳送至外部伺服器，則您會想要儘快知道。可監視資料傳送的自訂工具可以偵測到發現項目，並將它傳送給 {{site.data.keyword.security-advisor_short}}。如果您有另一個偵測到問題的自訂整合，則它也會將發現項目傳送至 {{site.data.keyword.security-advisor_short}}。然後，{{site.data.keyword.security-advisor_short}} 會顯示單一儀表板中所有監視工具的發現項目。您可以快速查看任何警示的概觀、調查問題，以及瞭解如何採取補救步驟。

範例有效負載：

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
