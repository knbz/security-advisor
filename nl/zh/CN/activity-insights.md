---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

---

{:new_window: target="_blank"}
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


# Activity Insights（预览）
{: #activity}

通过 {{site.data.keyword.security-advisor_long}}，您可以使用 {{site.data.keyword.cloud_notm}} Activity Tracker 来检测 {{site.data.keyword.cloud_notm}} 帐户中的可疑用户活动。
{: shortdesc}


## 工作方式
{: #activity-how}

Activity Insights 功能是 {{site.data.keyword.security-advisor_short}} 服务的一个附加组件。启用并配置此功能后，系统会记录并分析用户行为，以根据规则来识别可疑活动。您可以使用缺省规则，也可以创建适合您组织的定制规则。

请查看下图以了解信息流。

![Activity Insights 流程图](images/activity-insights-flow.png)

1. 作为帐户管理员，您可以将 Activity Insights 安装到集群中。
2. 通过将该附加组件安装到一个集群中，它可以监视整个帐户的 Activity Tracker 日志。
3. 活动日志会转发到 Cloud Object Storage 存储区并在其中进行存储，直到您决定将这些日志删除为止。使用 {{site.data.keyword.security-advisor_short}} GUI 来创建存储区时，系统会分配服务到服务 IAM 角色，以便该服务可以查看日志。
4. 在启用了 Activity Insights 的情况下，系统会根据规则来分析 COS 存储区中的原始数据，规则可由服务预定义，也可由您进行定制。
5. 在标记了一个可能的安全问题后，会将发现结果转发到“发现结果”数据库。
6. 发现结果会显示在服务仪表板中的 **Activity Insights** 卡上。

</br>

## 收集数据
{: #activity-data}

Activity Tracker 可收集用于描述用户与 {{site.data.keyword.cloud_notm}} API 进行的交互的事件。然后，可以将日志存储在 Object Storage 存储区中，以供进一步分析。
{: shortdesc}

Activity Tracker 可收集用于描述用户与 {{site.data.keyword.cloud_notm}} API 进行的交互的事件。

收集的信息包括：

* API 调用的发起方的 IP 地址
* 已认证的用户
* 活动类型
* 活动结果
* 等等...

收集的原始数据会存储在 Cloud Object Storage 存储区中，您可以确定数据在其中存储的时间长度。您对收集的数据具有所有权和控制权，这意味着存储、保护和删除这些数据将由您负责。{{site.data.keyword.security-advisor_short}} 会将发现结果保留 90 天。在此期间，结果会显示在服务仪表板中的 **Activity Insights** 卡上。因此，尽管 90 天后在仪表板中无法再看到发现结果，但原始数据仍可能位于存储器中。

从安全角度来说，如果法律或公司需求允许删除收集的数据，那么通常最好是清除这些数据。有关更多信息，请查看[删除对象](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion)。
{: tip}

## Activity Insights：访问权
{: #ai-access}

服务仪表板中的 Activity Insights 卡按用户和服务概述了意外或警报帐户活动的任何指示。不正常的活动可能是合法用户和服务的行为不当，也可能表明您的帐户已泄露。在卡上看到的发现结果是根据 {{site.data.keyword.security-advisor_short}} 提供的缺省规则包来确定的。

该卡引入了两个关键风险指标 (KRI)：

* 身份和访问权：与 Identity and Access Management (IAM) 或 {{site.data.keyword.appid_short_notm}} 服务相关的发现结果。
* 数据和 Kubernetes：与 Key Protect、Kubernetes Service、Cloud Object Storage 或 Certificate Manager 相关的发现结果。


## 了解规则包
{: #activity-packages}

作为帐户管理员，您可以利用规则包来快速开始监视帐户。
{: shortdesc}

该服务提供了与多个服务相关联的规则包，包括以下服务：

* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)
* {{site.data.keyword.cloudcerts_long_notm}}
* {{site.data.keyword.appid_long_notm}}
* {{site.data.keyword.keymanagementservicelong_notm}}
* {{site.data.keyword.cos_full_notm}} (COS)

如果当前包不满足您的所有需求，那么您可以随时更新现有包或创建新包，以定制适用于您组织的规则。

### 什么是规则？
{: #ai-rule}

规则是条件和单个事件的组合。可以使用规则或规则组合来触发可在 {{site.data.keyword.security-advisor_short}} 仪表板中显示的发现结果。

示例：

```
{
	"comment": "Dormant Rule: Very high risk {{site.data.keyword.appid_short_notm}}  activity... ",
		"dormant": true,
		"conditions": { 	… },
		"event": { … }
		"type": "aggregate"
	}
```
{: screen}

除了条件和事件外，规则还可以包含下表中的字段。

<table>
	<tr>
		<th colspan=2><img src="images/idea.png" alt="灯泡图标"/> 了解规则的组成部分</th>
	</tr>
	<tr>
		<td><code>comment</code></td>
		<td>始终忽略。</td>
	</tr>
	<tr>
		<td><code>dormant</code></td>
		<td>布尔值字段，值为 true 时，将忽略规则。如果值为 false 或 undefined，将使用规则。</td>
	</tr>
	<tr>
		<td><code>type</code></td>
		<td>选项包括：<code>aggregate</code>、<code>coincident</code> 和 <code>boolean</code>。如果为 type 分配的不是 <code>aggregate</code> 或 <code>coincident</code>，那么会将其作为 <code>boolean</code> 求值。</td>
	</tr>
</table>

</br>

### 什么是条件？
{: #ai-condition}

基本条件是一个构建块，由三个组成部分构成。这些块使用 `any` 和 `all` 运算符定界，并且可以嵌套。`all` 运算符相当于 `and`，`any` 相当于 `or`。

示例：

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
		<th colspan=2><img src="images/idea.png" alt="灯泡图标"/> 了解条件的组成部分</th>
	</tr>
	<tr>
		<td><code>fact</code></td>
		<td>正在检查的 Activity Tracker CADF 事件。</td>
	</tr>
	<tr>
		<td><code>operator</code></td>
		<td>选项包括：<code>equal</code>、<code>notEqual</code>、<code>lessThan</code>、<code>greaterThan</code>、<code>in</code> 和 <code>notIn</code></td>
	</tr>
	<tr>
		<td><code>value</code></td>
		<td>操作的定义方式。value 通常对应于可用于与服务进行交互的 API 调用。</td>
	</tr>
</table>

</br>

### 什么是事件？
{: #ai-event}

事件由两个字段组成：`type` 和 `params.findingType`。第一个字段是规则的唯一标识，而 `params.findingType` 是发出给服务的发现结果的名称。发现结果名称支持在 {{site.data.keyword.security-advisor_short}} 仪表板上显示相应发现结果。

示例：

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


### 规则类型：aggregate
{: #rule-aggregate}

aggregate 规则类型对特定时间范围内某个操作的发生次数进行计数，然后在计数超过定义的阈值时触发发现结果。规则可通过向布尔条件添加阈值和时段来进行定义。为了定义规则，必须满足以下多个条件。

* 规则类型必须为 `aggregate`。
* 根条件必须包含以下事实：

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

	下面是一些澄清：
	* X = 非零正整数
	* 如果选择小时，最大值可以为 24
	* 如果选择分钟，最大值可以为 1440。

#### 示例
{: #aggregate-example}

以下示例说明了在 30 分钟内对 5 次失败的尝试进行计数的规则：

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

### 规则类型：coincident
{: #rule-coincident}

coincident 规则类型监视操作，以了解同一操作在一段时间内发生的次数。规则可通过向一组基本条件构建块添加时段来进行定义。操作发生的顺序在 coincident 规则中无关紧要，但是为了定义规则，必须满足以下多个条件。

* 规则类型必须为 `coincident`。
* 根条件必须属于 `all` 种类。
* 根条件必须包含以下事实：

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

	下面是一些澄清：
	* `fact` 值必须为复数 `actions`，而不能为 `action`。
	* X = 非零正整数
	* 如果选择小时，最大值可以为 24
	* 如果选择分钟，最大值可以为 1440。


#### 示例
{: #coincident-example}

以下示例说明了用于监控三个特定操作必须在一个 30 分钟时间段内发生的同时性的规则：

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

### 规则类型：boolean
{: #rule-boolean}

boolean 规则由布尔条件和事件组成。布尔规则经常用于监视高风险 API 使用情况、非更改控制时段的 API 使用情况，或不在白名单中的发起方的 API 使用情况。

如果规则未定义为 `aggregate` 或 `coincident`，那么会将其作为 `boolean` 规则求值。

#### 示例
{: #boolean-example}

以下示例说明了用于监控非白名单用户在非更改控制时段内的策略删除操作的规则：

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

要了解有关 boolean 规则的更多信息吗？请查看 [Cache-Control 文档](https://github.com/CacheControl/json-rules-engine/blob/master/docs/rules.md){: external}。
{: tip}

## 后续步骤
{: #activity-next}

准备好开始了吗？请查看[启用 Activity Insights](/docs/services/security-advisor?topic=security-advisor-setup-activity)！
