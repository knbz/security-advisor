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


# Adding rule packages
{: #add-rule}

As an account administrator, you can create your own rule packages or download a predefined package that can be used to trigger a finding.
{: shortdesc}

Not sure how activity insights and rules fit together? Check out [Activity insights](activity-insights.html).
{: tip}

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

</br>

## Understanding rule types
{: #rule-types}

There are three different types of rules that can be set in Security Advisor: `aggregate`, `coincident`, and `boolean`.
{: shortdesc}


### Aggregate rules
{: #aggregate}

An aggregate rule type counts the number of occurrences of an action in a specific time frame and then triggers a finding if the defined threshold is exceeded. The rule is defined by adding the threshold and time window to the boolean condition. There are several conditions that must be met in order for the rule to be defined.

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

### Coincident rules
{: #coincident}

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

The following example demonstrates a rule that watches for a coincident of three specific actions that must occur within one 30 minute period:

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

### Boolean rules
{: #boolean}

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

Want to learn more about boolean rules? Check out <a href="https://github.com/CacheControl/json-rules-engine/blob/master/docs/rules.md" target="_blank">the CacheControl docs <img src="../../icons/launch-glyph.svg" alt="External link icon"></a>.
{: tip}

## Creating a package
{: #creating}

You can create a package by combining rules, a finding, and an event.  When the package is uploaded to the service, it is validated to ensure that each rule follows the correct syntax.

Account admins may install a rule package into Security Advisor COS bucket under "IBM.rules/activities" [add how to find the SA bucket name]

SHAWNA: WHEN DOES THE MACHINE DO THAT? WHAT DO THEY HAVE TO DO TO MAKE IT HAPPEN?

SHAWNA: HOW DO I CREATE THE INITIAL FILE? DO WE HAVE AN EXAMPLE FILE OR SOMETHING TO GET THEM STARTED? IS THE FILE ONLY PART OF THE PACKAGE? WHAT IS THE REST OF IT?

1. Add another rule to a list of rules in the JSON file.
	SHAWNA: INPUT EXAMPLE
2. To view the [finding](link) in your service dashboard, add a finding with the same finding ID.
	SHAWNA: INPUT EXAMPLE
