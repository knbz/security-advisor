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


# 自訂
{: #setup_custom}

{{site.data.keyword.security-advisor_short}} 可讓您整合現有的自訂安全工具，不論是開放程式碼、自訂已開發還是協力廠商服務。您可以將 URL 新增至整合清單，以建立從 {{site.data.keyword.security-advisor_short}} 到另一個工具的直接連線。您也可以整合協力廠商發現項目，以將嚴重安全事件直接帶至儀表板中的新卡片，方法是使用 API 來配置 API 及關鍵風險指標 (KRI)。
{: shortdesc}


## 新增直接連線
{: #setup-custom-gui}

您可以使用 {{site.data.keyword.security-advisor_short}} 儀表板，輕鬆地追蹤安全工具。
{: shortdesc}

### 開始之前
{: #custom-before-gui}

您必須先有要整合之夥伴的帳戶，才能新增整合。

{{site.data.keyword.security-advisor_short}} 不會持續保存任何與夥伴服務相關的認證。企業使用者必須使用 SAML 向 {{site.data.keyword.Bluemix_notm}} 及事業夥伴進行鑑別。
{: note}

### 配置連線
{: #custom-configure-connection}

1. 登入安全工具，並取得唯一的 URL。

2. 使用主控台登入 {{site.data.keyword.cloud_notm}}。

3. 按一下**自訂整合 > 直接連線**。即會顯示畫面。

  1. 為解決方案提供名稱。名稱中只能使用英數字元、空格及橫線 (-)。

  2. 輸入解決方案的 URL，格式為：`www.<website>.<domain>`。

  3. 上傳代表該工具的圖示或影像。

  4. 按一下**連接**來完成配置。{{site.data.keyword.security-advisor_short}} 會建立整合所需的構件，例如服務 ID、API 金鑰、帳戶 ID 及 meta 資料。已指派 `writer` 角色。



## 整合協力廠商發現項目

這些 API 遵循構件 meta 資料規格這類的 Grafeas，來儲存、查詢與擷取安全工具及服務所報告之發現項目的嚴重 meta 資料。
{: #setup-custom-api}

### 開始之前
{: #custom-before-api}

整合來自協力廠商工具的發現項目之前，請確定您具有下列必要條件。

1. 確定您使用的使用者或服務 ID 已獲指派**管理員** [IAM 角色](https://cloud.ibm.com/iam#/users)。

1. 登入 {{site.data.keyword.cloud_notm}}。

  ```
  ibmcloud login
  ```
  {: codeblock}

2. 取得帳戶 ID。如需服務角色的相關資訊，請查看 [{{site.data.keyword.security-advisor_short}}存取原則](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)。

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

3. 取得 Identity and Access Management (IAM) 記號。記號用於每個 API 要求的 `--header` 中。

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  IAM 記號會每 60 分鐘到期一次。瞭解如何使用 API 金鑰來[自動取得新的記號](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey)。
  {: tip}



### 匯入發現項目及 KRI
{: #custom-adding}

1. 建立附註來登錄新的「發現項目」類型。若要建立附註，請使用 [Findings API](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote)。請確定您選擇唯一的提供者 ID 來識別自訂工具。如果您使用服務 ID 將處理程序自動化，則可以使用服務 ID 作為提供者 ID。

  範例要求：

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Content-Type: application/json" -d "{ \"kind\": \"FINDING\", \"short_description\": \"My security tool finding\", \"long_description\": \"See what my custom security tool found\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-findings-type\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Custom Security Tool\" }, \"finding\": { \"severity\": \"MEDIUM\", \"next_steps\": [ { \"title\": \"Learn why this is reported as a risk\" } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="「相關資訊」圖示"/> 瞭解這個 commands 元件 </th>
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
        <td>解決問題所需採取的步驟。</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>自訂安全工具。</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>安全工具所找到的發現項目類型 ID。</td>
      </tr>
      <tr>
        <td><code>context</code><ul><li><code>region</code></li><li><code>resource_id</code></li> <li><code>resource_name</code></li> <li><code>resource_type</code></li> <li><code>service_name</code></li></ul></td>
        <td></br><ul><li><code>發生發現項目的位置</code></li><li><code>特定資源的 ID。</code></li> <li><code>資源的名稱。</code></li> <li><code>資源的類型。</code></li> <li><code>服務的名稱。</code></li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>url</code></li></ul></td>
        <td></br><ul><li>發現項目所呈現的緊急程度。</li> <li>重新修補問題時可以採取的步驟。</li> <li>可以找到發現項目詳細資料的 URL。</li></ul></td>
      </tr>
    </tbody>
  </table>

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

2. 將發現項目公佈為 KRI 或事件，否則稱為[出現項目](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence)。

  您可以針對每張卡片，定義兩個 KRI。
  {: note}

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account-id>/providers/my-custom-tool/occurrences" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Replace-If-Exists: true" -H "Content-Type: application/json" -d "{ \"note_name\": \"<account-id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\", \"kind\": \"FINDING\", \"remediation\": \"how to resolve this\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-finding-1\", \"context\": { \"region\": \"location\", \"resource_id\": \"pluginId\", \"resource_name\": \"www.myapp.com\", \"resource_type\": \"worker\", \"service_name\": \"application\" }, \"finding\": { \"severity\": \"HIGH\", \"next_steps\": [{ \"url\": \"Details URL\" }] }}"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="「相關資訊」圖示"/> 瞭解這個 commands 元件 </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>彙總發現項目的簡要說明；不得超過幾個單字。</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>包含發現項目詳細資料的較詳細說明。</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>自訂安全工具。</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>安全工具所找到的發現項目類型 ID。</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>已報告發現項目的安全工具 ID。</li><li>已報告發現項目的安全工具標題。</li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>title</code></li></ul></td>
        <td></br><ul><li>發現項目所呈現的緊急程度。</li> <li>重新修補問題時可以採取的步驟。</li> <li>發現項目的標題。</li></ul></td>
      </tr>
    </tbody>
  </table>

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

3. 建立[附註](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote)，在儀表板中定義卡片以顯示發現項目。

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam token>" -H "Content-Type: application/json" -d "{ \"kind\": \"CARD\", \"provider_id\": \"my-custom-tool\", \"id\": \"custom-tool-card\", \"short_description\": \"security risk found by my custom tool\", \"long_description\": \"Details about why this is security risk to be fixed\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Security Tool\" }, \"card\": { \"section\": \"My Security Tools\", \"title\": \"My Security Tool Findings\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ], \"elements\": [ { \"kind\": \"NUMERIC\", \"text\": \"Count of findings reported by my security tool\", \"default_time_range\": \"1d\", \"value_type\": { \"kind\": \"FINDING_COUNT\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ] } } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="「相關資訊」圖示"/> 瞭解這個 commands 元件 </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>CARD</code></td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>自訂安全工具。</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>安全工具所找到的發現項目類型 ID。</td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>彙總發現項目的說明，不得超過幾個單字。</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>包含發現項目詳細資料的較詳細說明。</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>已報告發現項目的安全工具 ID。</li><li>已報告發現項目的安全工具標題。</li></ul></td>
      </tr>
      <tr>
        <td><code>card</code> <ul><li><code>section</code></li> <li><code>title</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>卡片所適用的區段。</li> <li>卡片的標題</li> <li><code>providers/<provider_id>/notes/my-custom-tool-findings-type</code></li></ul></td>
      </tr>
      <tr>
        <td><code>elements</code> <ul><li><code>kind</code></li> <li><code>text</code></li> <li><code>default_time_range</code></li></ul></td>
        <td></br><ul><li>元素的類型。</li> <li>您要顯示的文字</li> <li>您要檢查的時間量。</li></ul></td>
      </tr>
      <tr>
        <td><code>value_type</code> <ul><li><code>kind</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>值的類型</li> <li>您要在卡片中看到的發現項目名稱。</li></ul></td>
      </tr>
    </tbody>
  </table>

  範例回應：
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

4. 導覽至服務儀表板，以查看您已建立的卡片。

## 範例用法
{: #custom-example}

假設，您的應用程式在具有名稱 `cloudkingdom` 的 {{site.data.keyword.containershort_notm}} 叢集上執行。根據應用程式大小，在叢集內可能有數個要同時監視的 Pod。如果您有多個自訂工具可監視並偵測叢集是否有不同的威脅，則要怎麼做。如果叢集中的其中一個 Pod 開始將異常數量的資料傳送至外部伺服器，則您會想要儘快知道。可監視資料傳送的自訂工具可以偵測到發現項目，並將它傳送給 {{site.data.keyword.security-advisor_short}}。如果您有另一個偵測到問題的自訂整合，則它也會將發現項目傳送至 {{site.data.keyword.security-advisor_short}}。然後，{{site.data.keyword.security-advisor_short}} 會顯示單一儀表板中所有監視工具的發現項目。您可以快速查看任何警示的概觀、調查問題，以及瞭解如何採取補救步驟。


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
                        "url":"https://console.bluemix.net/containers-kubernetes/clusters"
		}],
                "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
                "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod’s normal behavior"
	}
}
```
{: screen}
