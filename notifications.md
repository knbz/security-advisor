<staging>---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-22"

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


# Configuring notifications
{: #notifications}


By configuring an IBM Cloud Security Advisor notification channel you can be alerted to any reported vulnerabilities as soon as the report is available. With a fast alert time, you're able to immediately start an investigation into any reported issue and fix the vulnerability before it becomes a larger problem in your application. 
{: shortdesc}

By defining a process for handling alerts, you can ensure that you're in compliance and prepared if/when there is an issue. For example, some compliance standards require that issues must be responded to and closed within 24 hours, and the response is audited. With Security Advisor alerts in place, you are notified and can begin to resolve issues immediately.


## Before you begin
{: #notifications-before}

Before you get started with notifications, you must be assigned the proper IAM roles. Check out the following table to see which roles you need to perform which actions. For more information about roles, see [managing service access](/docs/services/security-advisor?topic=security-advisor-service-access).

<table>
  <tr>
    <td>Action</td>
    <td>Reader</td>
    <td>Manager</td>
  </tr>
  <tr>
    <td>List channels</td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td>Download public key</td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td>Test channel connection</td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td>Create a channel</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td>Update a channel</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td>Delete a channel</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
</table>

## Configuring a notification method
{: #notification-method}


## Setting up a Slack Webhook
{: #notification-slack}

To set up a Slack Webhook, complete the following steps:

1. Sign up for [Slack](https://slack.com/) and set up your workspace.
2. Create a Slack channel where you want to post your notifications to.
3. [Set up a Webhook](https://api.slack.com/incoming-webhooks) for the Slack channel.



### Setting up a Callback URL
{: #notification-callback}

You can use a Callback URL to post notifications to the tools that you use. For example, you can send notifications to report to PagerDuty, automatically open an issue in GitHub, or trigger renewal scripts.
{: shortdesc}

**Important:** Your Callback URL endpoint must meet the following requirements:
* The endpoint must use the HTTPS protocol.
* The endpoint must not require HTTP headers. This requirement includes authorization headers.
* The endpoint must return a `200 OK` status code to indicate a successful notification delivery.



## Creating a notification channel

### With the GUI

1. Navigate to the **Notifications channels** tab of the Security Advisor dashboard.
2. Click **Add notification channel**.
3. Fill in the following information.

  <table>
    <tr>
      <th colspan= "2">Notification configurations</th>
    </tr>
    <tr>
      <td>Name</td>
      <td>The name of the channel.</td>
    </tr>
    <tr>
      <td>Description</td>
      <td>Describe what the channel is used for. For example: "This channel sends high severity notifications as they happen."</td>
    </tr>
    <tr>
      <td>Method</td>
      <td>Select the method that you're using to send the notifications. Options include Callback URL and Slack WebHook.</td>
    </tr>
    <tr>
      <td>Channel endpoint</td>
      <td>The location that you want to be notified. Examples include a slack channel, an email address, or a PagerDuty service.</td>
    </tr>
    <tr>
      <td>Severity</td>
      <td>The severity of the notifications that you want to send to be notified about.</td>
    </tr>
    <tr>
      <td>Frequency</td>
      <td>The frequency in which you want to be notified about issues. Options include as they happen and every 15 minutes.</td>
    </tr>
  </table>

3. Click **Save**.


### With the API


## Testing the connection

-	Can test the connection with the webhook/callback url being provided for a particular notification channel.

### With the GUI


### With the API



## Verifying the payload


When a notification is sent, you can use a public key to decrypt and verify the payload that is received. By using a public key, you can ensure that the information in the payload has not been tampered with in any way.

### With the GUI


### With the API

When a notification is sent, you can use a public key to decrypt and verify the payload that is received. By using a public key, you can ensure that the information in the payload has not been tampered with in any way. 


-	User can download public key, which can be used to decrypt/verify the payload being received on the callback url provided during notification creation.
-	User will be able to view all the notification channel created in his account.


```
curl -X GET "https://us-south.secadvisor.cloud.ibm.com/notifications/v1/{}/download_public_key" -H "accept: application/json" -H "Authorization: {IAM-token}"
```
{: codeblock}


## Deleting a notification channel





### With the GUI


### With the API

Note: Want to take a break from receiving notifications but don't want to delete your configuration? No problem, disable your channel configuration instead. Then, when you're ready to use the configuration again, you can just flip the switch to enabled and you're ready to go!

-	Delete any notification channel/channels.
-	Can enable/disable the notification channels.




Security Advisor Notification Channel API (https://security-advisor-dev.us-south.containers.appdomain.cloud/notifications/v1/docs) :
-	Provides all set of publicly exposed API will a user can call directly or will be called from SA dashboard during any action on Notification channels page.
-	API’s include creation of channel, List all channels, Delete channel/s, Download public key/ Test channel connection etc.







