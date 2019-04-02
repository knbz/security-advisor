---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-14"

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

# Activity Insights（預覽）
{: #activity}

使用 {{site.data.keyword.security-advisor_long}}，您可以使用 {{site.data.keyword.cloud_notm}} Activity Tracker 來偵測 {{site.data.keyword.Bluemix_notm}} 帳戶中的可疑使用者活動。
{: shortdesc}


## 如何運作
{: #activity-how}

Activity Insights 特性是 {{site.data.keyword.security-advisor_short}} 服務的附加程式。啟用並配置此特性之後，會記載並分析使用者行為，以根據規則來識別可疑活動。您可以使用預設規則，也可以建立自訂規則以適合您的組織。

請查看下列影像，以查看資訊流程。

![Activity Insights 流程圖](images/activity-insights-flow.png)

1. 身為帳戶管理者，您可以將 Activity Insights 安裝到叢集。
2. 將附加程式安裝至某個叢集時，即可監視整個帳戶的 Activity Tracker 日誌。
3. 除非您決定刪除活動日誌，否則會將活動日誌轉遞至其儲存所在的 Cloud Object Storage 儲存區。當您使用 {{site.data.keyword.security-advisor_short}} GUI 來建立儲存區時，會指派服務對服務 IAM 角色，讓服務可以檢視日誌。
4. 啟用 Activity Insights 時，會根據服務可預先定義或由您自訂的規則來分析 COS 儲存區中的原始資料。
5. 標示可能的安全問題時，會將發現項目轉遞至「發現項目」資料庫。
6. 發現項目會顯示在服務儀表板的 **Activity Insights** 卡片上。

</br>

## 收集資料
{: #activity-data}

Activity Tracker 會收集可說明針對 {{site.data.keyword.cloud_notm}} API 的使用者互動的事件。接著，您可以將日誌儲存至 Object Storage 儲存區，以進一步分析。
{: shortdesc}

Activity Tracker 會收集可說明針對 {{site.data.keyword.cloud_notm}} API 的使用者互動的事件。

所收集的資訊包括：

* API 呼叫起始器的 IP 位址
* 已鑑別的使用者
* 活動類型
* 活動結果
* 及其他項目...

所收集的原始資料儲存至 Cloud Object Storage 儲存區，您可以在其中判斷其儲存時間長度。您擁有並控制所收集的資料，這表示您負責儲存、保護及刪除它。{{site.data.keyword.security-advisor_short}} 會維護發現項目 90 天。在這個期間，結果會呈現在服務儀表板的 **Activity Insights** 卡上。因此，雖然您在 90 天之後就不會再於儀表板中看到發現項目，但在儲存空間中可能仍然有原始資料。

從安全觀點來看，在法律或公司需求容許刪除所收集的資料時，通常最好予以清除。如需相關資訊，請查看[刪除物件](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion)。
{: tip}

## Activity Insights：存取
{: #ai-access}

服務儀表板中的 Activity Insights 卡會彙總使用者及服務的所有非預期或警示帳戶活動的指示。合法使用者及服務可能會因活動異常而行為錯誤，或者可能也表示您的帳戶受損。您在該卡片上看到的發現項目是根據 {{site.data.keyword.security-advisor_short}} 提供的預設規則套件來判定。

該卡片引進兩個「關鍵風險指標 (KRI)」：

* 身分及存取：與 Identity and Access Management (IAM) 或 App ID 服務相關的發現項目。
* 資料及 Kubernetes：與 Key Protect、Kubernetes Service、Cloud Object Storage 或 Certificate Manager 相關的發現項目。


## 瞭解規則套件
{: #activity-packages}

身為帳戶管理者，您可以利用規則套件來快速開始監視帳戶。
{: shortdesc}

此服務提供與數個服務相關聯的規則套件，包括：

* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM)
* {{site.data.keyword.cloudcerts_long_notm}}
* {{site.data.keyword.appid_long_notm}}
* {{site.data.keyword.keymanagementservicelong_notm}}
* {{site.data.keyword.cos_full_notm}} (COS)

如果現行套件不符合您的所有需求，則一律可以更新現有的套件，或建立新的套件，以修改您組織的規則。

### 何謂規則？
{: #ai-rule}

規則是一個條件與單一事件的組合。您可以使用規則或規則組合來觸發可在 {{site.data.keyword.security-advisor_short}} 儀表板中顯示的發現項目。

範例：

```
	{
		"comment": "Dormant Rule: Very high risk App ID activity... ",
		"dormant": true,
		"conditions": { 	… },
		"event": { … }
		"type": "aggregate"
	}
```
{: screen}

除了條件及事件之外，規則還可以包含下表中所找到的欄位。

<table>
	<tr>
		<th colspan=2><img src="images/idea.png" alt="燈泡圖示"/> 瞭解規則的元件</th>
	</tr>
	<tr>
		<td><code>comment</code></td>
		<td>一律忽略。</td>
	</tr>
	<tr>
		<td><code>dormant</code></td>
		<td>布林欄位，若為 true，則予以忽略。如果值為 false 或未定義，則會使用規則。</td>
	</tr>
	<tr>
		<td><code>type</code></td>
		<td>選項包括：<code>aggregate</code>、<code>coincident</code> 及 <code>boolean</code>。如果未指派類型 <code>aggregate</code> 或 <code>coincident</code>，則會將它評估為 <code>boolean</code>。</td>
	</tr>
</table>

</br>

### 何謂條件？
{: #ai-condition}

基本條件是由三個元件組成的構成要素。這些構成要素是使用 `any` 及 `all` 運算子所連結，而且可以是巢狀的。`all` 運算子相當於 `and`，而 `any` 相當於 `or`。

範例：

```
	"conditions": {
		"all": [{
			"any": [{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.group.delete"
			},
			{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.member.delete"
			}]
		}
	}
```
{: screen}

<table>
	<tr>
		<th colspan=2><img src="images/idea.png" alt="燈泡圖示"/> 瞭解條件的元件</th>
	</tr>
	<tr>
		<td><code>fact</code></td>
		<td>正在檢查的 Activity Tracker CADF 事件。</td>
	</tr>
	<tr>
		<td><code>operator</code></td>
		<td>選項包括：<code>equal</code>、<code>notEqual</code>、<code>lessThan</code>、<code>greaterThan</code>、<code>in</code> 及 <code>notIn</code></td>
	</tr>
	<tr>
		<td><code>value</code></td>
		<td>定義動作的方式。此值通常對應於可用來與服務互動的 API 呼叫。</td>
	</tr>
</table>

</br>

### 何謂事件？
{: #ai-event}

事件由兩個欄位組成：`type` 及 `params.findingType`。第一個是規則的唯一 ID，而 `params.findingType` 是發給服務的發現項目名稱。發現項目名稱容許在 {{site.data.keyword.security-advisor_short}} 儀表板上顯示發現項目。

範例：

```
	{
		"conditions": { 	… },
		"event": {
			"type": "IKS high risk API",
			"params": {"findingType": "IKS-high-risk"}
		}
	}
```
{: screen}


### 規則類型：aggregate
{: #rule-aggregate}

aggregate 規則類型會計算特定時間範圍中某個動作的出現次數，然後在超出所定義的臨界值時觸發發現項目。透過將臨界值及時間範圍新增至布林條件來定義規則。必須符合數個條件，才能定義規則。

* 規則類型必須是 `aggregate`。
* 根條件必須包含下列 fact：

	```
	{
			"fact": "occurrences",
			"operator": [equal | greaterThan | greaterThanInclusive],
			"value": [positive number]
	},
	{
	    "fact": "withInLast",
	    "operator": "equal",
	    "value": "X [minutes|hours]",
	}
	```
	{: screen}

	部分釐清：
	* X = 到非零正整數
	* 選取小時時，最大值可以是 24
	* 選取分鐘時，最大值可以是 1440。

**範例**

下列範例示範在 30 分鐘內嘗試失敗五次的規則：

```
{
    "conditions": {
        "all": [
            {
                "fact": "action",
                "operator": "equal",
                "value": "iam-identity.user-apikey.login"
            },
            {
                "fact": "reason",
                "operator": "equal",
                "value": 400,
                "path": ".reasonCode"
            },
            {
                "fact": "occurrences",
                "operator": "equal",
                "value": 5
            },
            {
                "fact": "withInLast",
                "operator": "equal",
                "value": "30 minutes",
            }
        ]
    },
    "event": {
        "type": "failed-login-attempts",
        "params": {
            "findingType": "failed-login-attempts",
        }
    },
    "type" : "aggregate"
}
```
{: screen}

### 規則類型：coincident
{: #rule-coincident}

coincident 規則類型會監視動作，以查看相同動作在一段時間範圍內的出現次數。規則是透過將時間範圍新增至一組基本條件構成要素來定義。動作的發生順序在 coincident 規則中沒有顯著性，但必須符合數個條件，才能定義規則。

* 規則類型必須是 `coincident`。
* 根條件必須為 `all` 變化。
* 根條件必須包含下列 fact：

	```
	{
	    "fact": "actions",
	    "operator": "contains",
	    "value": [action value]
	},
	{
	    "fact": "withInLast",
	    "operator": "equal",
	    "value": "X [minutes|hours]",
	}
	```
	{: screen}

	部分釐清：
	* `fact` 值必須是複數：`actions`，而非 `action`。
	* X = 到非零正整數
	* 選取小時時，最大值可以是 24
	* 選取分鐘時，最大值可以是 1440。


**範例**

下列範例所示範的規則監看必須在三十分鐘期間內發生的三個特定動作的 coincident：

```
{
    "conditions": {
        "all": [
            {
                "fact": "actions",
                "operator": "contains",
                "value": "iam-identity.user-apikey.login"
            },
            {
                "fact": "actions",
                "operator": "contains",
                "value": "kms.secrets.list"
            },
            {
                "fact": "actions",
                "operator": "contains",
                "value": "kms.secrets.read"
            },
            {
                "fact": "withInLast",
                "operator": "equal",
                "value": "30 minutes",
            }
        ]
    },
    "event": {
        "type": "key-protect-keys-read",
        "params": {
            "findingType": "key-protect-keys-read",
        }
    },
    "type" : "coincident"
}
```
{: screen}

### 規則類型：boolean
{: #rule-boolean}

boolean 規則是由布林條件及事件所組成。boolean 規則經常用來監視高風險 API 用量、落在變更控制時間範圍之外的 API 用量，或未在白名單上的起始器的 API 用量。

如果規則未定義為 `aggregate` 或 `coincident`，則會將它評估為 `boolean` 規則。

**範例**

下列範例所示範的規則監看不在白名單上的使用者如何刪除落在變更控制時間範圍之外的原則：

```
{
	"conditions": {
		"all": [{
			"any": [{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.group.delete"
			},
			{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.member.delete"
			}]
		},
		{
			"any": [{
				"fact": "event_time",
				"operator": "lessThan",
				"value": "0800"
			},
			{
				"fact": "event_time",
				"operator": "greaterThan",
				"value": "1700"
			}
			]
		},
		{
			"fact": "initiator",
			"path": ".name",
			"operator": "notIn",
			"value": ["example@ibm.com", "example2@ibm.com"]
		}
		]
	},
	"event": {
		"type": "account-delete",
		"params": {
			"findingType": "iam-delete-account-threshold"
		}
	}
```
{: screen}

要進一步瞭解 boolean 規則嗎？請查看 <a href="https://github.com/CacheControl/json-rules-engine/blob/master/docs/rules.md" target="_blank">CacheControl 文件 <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。
{: tip}

## 後續步驟
{: #activity-next}

準備好要開始了嗎？請查看[啟用 Activity Insights](/docs/services/security-advisor?topic=security-advisor-setup-activity)！
