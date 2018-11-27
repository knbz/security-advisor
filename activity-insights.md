---

copyright:
  years: 2018
lastupdated: "2018-11-27"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Activity insights (experimental)
{: #activity}

With {{site.data.keyword.security-advisor_long}}, you can gain insight into potentially hazardous user behavior and compromised accounts. As an account administrator, you can gain insights based on best practices as well as custom made rules that are set by you.
{: shortdesc}

The activity insights feature is offered as an add-on to the {{site.data.keyword.security-advisor_short}} service and is currently experimental. With the feature enabled and installed, user activity across your IBM Cloud services is collected and continuously monitored and analyzed.

**How does it work?**

Activity insights rely on the collection of {{site.data.keyword.cloudaccesstrailshort}} data through a collector offered by {{site.data.keyword.security-advisor_short}}. The data is collected to a Cloud Object Storage bucket where the service can analyze the events. Don't worry, you own and control the collected data. The results are then presented on the activity insights card in the {{site.data.keyword.security-advisor_short}} dashboard.

</br>

## Collecting data
{: #data}

{{site.data.keyword.security-advisor_short}} can help protect your cluster by adding access monitoring.
{: shortdesc}

Deployed to your cluster as a container, access monitoring is used to provide information about user activity. To enable insights, events that describe user interactions against {{site.data.keyword.Bluemix_notm}} APIs are collected by the {{site.data.keyword.cloudaccesstrailshort}} service.

The following information is collected:

* The IP address of the initiator of the API call
* The user that was authenticated
* The activity type
* The activity result
* and more...

All of the activity data that is collected is stored within an Object Storage bucket. As the account admin, you might access this data for forensics or to integrate additional analytics tools.

</br>

## Understanding rule packages
{: #packages}

As an account admin, you can quickly start monitoring your accounts by taking advantage of rule packages.
{: shortdesc}

The service offers rule packages that are associated with specific {{site.data.keyword.Bluemix_notm}} services including:

* {{site.data.keyword.containerlong_notm}}
* IBM Cloud Identity and Access Management (IAM)
* IBM Cloud Certificate Manager
* IBM Cloud App ID
* IBM Cloud Key Protect
* IBM Cloud Object Store (COS)


You can update existing or create new packages to tailor the rules to your organization's needs. Ready to get started? Check out [Adding rule packages](rules.html).
{: tip}


**What is a rule?**

A rule is the combination of conditions and a single event. You can use rules, or the combination of rules to trigger findings that can display in your Security Advisor dashboard.

Example:

```
	{
		"comment": "Dormant Rule: Very high risk AppID activity... ",
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

**What is a condition?**

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
		<td>SHAWNA: WHAT IS THIS?</td>
	</tr>
</table>

</br>

**What is an event?**

An event is composed of two fields: `type` and `params.findingType`. The first is a unique identifier for a rule, while `params.findingType` is the name of the finding that is issued to the service. The finding name allows for the finding to be displayed on the Security Advisor dashboard.

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
