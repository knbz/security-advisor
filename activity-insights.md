---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-23"

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

# Activity Insights (preview)
{: #activity}

With {{site.data.keyword.security-advisor_long}}, you can detect suspicious user activity in your {{site.data.keyword.cloud_notm}} account by using {{site.data.keyword.cloud_notm}} Activity Tracker.
{: shortdesc}


## How it works
{: #activity-how}

The Activity Insights feature is an add-on to the {{site.data.keyword.security-advisor_short}} service. With the feature enabled and configured, user behavior is logged and analyzed to identify suspicious activity based on rules. You can use default rules, or you can create custom rules to fit your organization.

Check out the following image to see the flow of information.

![Activity Insights flow diagram](images/activity-insights-flow.png)

1. An an account administrator, you can install Activity Insights into your cluster.
2. With the add-on installed into one cluster, it can monitor Activity Tracker logs for your entire account.
3. The activity logs are forwarded to a Cloud Object Storage bucket where they are stored until you decide to delete them. When you use the {{site.data.keyword.security-advisor_short}} GUI to create the bucket, service to service IAM roles are assigned so that the service can view the logs.
4. With Activity Insights enabled, the raw data in your COS bucket is analyzed based on rules that can be predefined by the service or customized by you.
5. When a possible security issue is flagged, the finding is forwarded to the Findings database.
6. Findings are displayed in your service dashboard on the **Activity Insights** card.

</br>

## Collecting data
{: #activity-data}

Activity Tracker collects events that describe user interactions against {{site.data.keyword.cloud_notm}} APIs. You can then store the logs in an Object Storage bucket for further analysis.
{: shortdesc}

Activity Tracker collects events that describe user interactions against {{site.data.keyword.cloud_notm}} APIs.

Collected information includes:

* The IP address of the initiator of the API call
* The user that was authenticated
* The activity type
* The activity result
* And more ...

The raw data that is collected is stored in a Cloud Object Storage bucket where you can determine the length of time that it is stored. You own and control the collected data, which means that you're responsible for storing, securing, and deleting it. {{site.data.keyword.security-advisor_short}} maintains findings for 90 days. During that time, the results are presented on the **Activity Insights** card in the service dashboard. So, although you will no longer see the finding in your dashboard after 90 days, you might still have the raw data in storage.

From a security point of view, it's generally a good idea to purge your collected data when legal or company requirements allow for it to be deleted. For more information, check out [Deleting objects](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion).
{: tip}

## Activity Insights: Access
{: #ai-access}

The Activity Insights card in the service dashboard summarizes any indication of unexpected or alarming account activity by your users and services. Activity out of the ordinary might be misbehavior by legitimate users and services, or it could be a sign that your account is compromised. The findings that you see on the card are determined based on the {{site.data.keyword.security-advisor_short}} provided default rule packages.

The card introduces two Key Risk Indicators (KRIs):

* Identity and Access: Findings that are related to the Identity and Access Management (IAM) or {{site.data.keyword.appid_short_notm}}  services.
* Data and Kubernetes: Findings that are related to Key Protect, Kubernetes Service, Cloud Object Storage, or Certificate Manager.


## Understanding rule packages
{: #activity-packages}

As an account admin, you can quickly start monitoring your accounts by taking advantage of rule packages.
{: shortdesc}

The service offers rule packages that are associated with several services including:

* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)
* {{site.data.keyword.cloudcerts_long_notm}}
* {{site.data.keyword.appid_long_notm}}
* {{site.data.keyword.keymanagementservicelong_notm}}
* {{site.data.keyword.cos_full_notm}} (COS)

If the current packages don't meet all of your needs, you can always update an existing package or create a new one to tailor the rules to your organization.

### What is a rule?
{: #ai-rule}

A rule is the combination of conditions and a single event. You can use rules, or the combination of rules to trigger findings that can display in your {{site.data.keyword.security-advisor_short}} dashboard.

Example:

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

In addition to a condition and an event, rules can also contain the fields that are found in the following table.

<table>
	<tr>
		<th colspan=2><img src="images/idea.png" alt="light bulb icon"/> Understanding the components of a rule</th>
	</tr>
	<tr>
		<td><code>comment</code></td>
		<td>Always ignored.</td>
	</tr>
	<tr>
		<td><code>dormant</code></td>
		<td>A Boolean field that if true, is ignored. If the value is false or undefined, the rule is used.</td>
	</tr>
	<tr>
		<td><code>type</code></td>
		<td>Options include: <code>aggregate</code>, <code>coincident</code>, and <code>boolean</code>. If the type is not assigned <code>aggregate</code> or <code>coincident</code>, then it is evaluated as <code>boolean</code>.</td>
	</tr>
</table>

</br>

### What is a condition?
{: #ai-condition}

A basic condition is a building block that is composed of three components. The blocks are bound by using `any` and `all` operators and can be nested. An `all` operator is the equivalent of `and` while `any` is equal to `or`.

Example:

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
		<th colspan=2><img src="images/idea.png" alt="light bulb icon"/> Understanding the components of a condition</th>
	</tr>
	<tr>
		<td><code>fact</code></td>
		<td>The Activity Tracker CADF event that is being inspected.</td>
	</tr>
	<tr>
		<td><code>operator</code></td>
		<td>Options include: <code>equal</code>, <code>notEqual</code>, <code>lessThan</code>, <code>greaterThan</code>, <code>in</code>, and <code>notIn</code></td>
	</tr>
	<tr>
		<td><code>value</code></td>
		<td>The way that an action is defined. The value usually corresponds to an API call that can be used to interact with the service.</td>
	</tr>
</table>

</br>

### What is an event?
{: #ai-event}

An event is composed of two fields: `type` and `params.findingType`. The first is a unique identifier for a rule, while `params.findingType` is the name of the finding that is issued to the service. The finding name allows for the finding to be displayed on the {{site.data.keyword.security-advisor_short}} dashboard.

Example:

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


### Rule type: aggregate
{: #rule-aggregate}

An aggregate rule type counts the number of occurrences of an action in a specific time frame and then triggers a finding if the defined threshold is exceeded. The rule is defined by adding the threshold and time window to the boolean condition. Several conditions must be met in order for the rule to be defined.

* The rule type must be `aggregate`.
* The root condition must contain the following facts:

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

	Some clarifications:
	* X = to a non-zero positive integer
	* When hours are selected, the max value can be 24
	* When minutes are selected, the max value can be 1440.

**Example**

The following example demonstrates a rule that counts five failed attempts within 30 minutes:

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

### Rule type: coincident
{: #rule-coincident}

A coincident rule type monitors actions to see how many times the same action occurs within a window of time. The rule is defined by adding a time window to a group of basic condition building blocks. The order that the actions occur has no significance in the coincident rule, but there are several conditions that must be met in order for the rule to be defined.

* The rule type must be `coincident`.
* The root condition must be of the `all` variety.
* The root condition must contain the following facts:

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

	Some clarifications:
	* The `fact` value must be plural: `actions` not `action`.
	* X = to a non-zero positive integer
	* When hours are selected, the max value can be 24
	* When minutes are selected, the max value can be 1440.


**Example**

The following example demonstrates a rule that watches for a coincident of three specific actions that must occur within one thirty-minute period:

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

### Rule type: boolean
{: #rule-boolean}

A boolean rule is composed of a boolean condition and an event. Boolean rules are frequently used to monitor high-risk API usage, API usage that falls outside of the change control window, or API usage by an initiator that is not on a whitelist.

If a rule is not defined as `aggregate` or `coincident`, then it is evaluated as a `boolean` rule.

**Example**

The following example demonstrates a rule that watches for the deletion of policy outside the change control window by a user that is not on the whitelist:

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

Want to learn more about boolean rules? Check out <a href="https://github.com/CacheControl/json-rules-engine/blob/master/docs/rules.md" target="_blank">the Cache-Control docs <img src="../../icons/launch-glyph.svg" alt="External link icon"></a>.
{: tip}

## Next steps
{: #activity-next}

Ready to get started? Check out [Enabling Activity Insights](/docs/services/security-advisor?topic=security-advisor-setup-activity)!
