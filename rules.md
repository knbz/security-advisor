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
