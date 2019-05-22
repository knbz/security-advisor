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


## Configuring notifications with the GUI

### Creating a notification channel

Creating a webhook:

-	User can create new notification channel (Only Webhook is supported as of now)






### Testing the connection

-	Can test the connection with the webhook/callback url being provided for a particular notification channel.




### Verifying the payload

When a notification is sent, you can use a public key to decrypt and verify the payload that is received. By using a public key, you can ensure that the information in the payload has not been tampered with in any way. 


-	User can download public key, which can be used to decrypt/verify the payload being received on the callback url provided during notification creation.
-	User will be able to view all the notification channel created in his account.

### Deleting a notification channel

Note: Want to take a break from receiving notifications but don't want to delete your configuration? No problem, disable your channel configuration instead. Then, when you're ready to use the configuration again, you can just flip the switch to enabled and you're ready to go!

-	Delete any notification channel/channels.
-	Can enable/disable the notification channels.


## Configuring notifications with the API



Security Advisor Notification Channel API (https://security-advisor-dev.us-south.containers.appdomain.cloud/notifications/v1/docs) :
-	Provides all set of publicly exposed API will a user can call directly or will be called from SA dashboard during any action on Notification channels page.
-	API’s include creation of channel, List all channels, Delete channel/s, Download public key/ Test channel connection etc.





What is Notification:
At the moment with Security Advisor, there is no means of letting user know about any vulnerabilities reported unless they visit the Security Advisor Dashboard or manually call the SA FindingsAPI. User/Customer/Security focal should be able to get notified on findings that occurs in their environment and should be able to act on them immediately/timely. So the purpose of introducing the Notification capability in Security Advisor is to send alerts related to findings vulnerability to the user based on the set configuration. Notification from all findings sources with any severity will be send to Webhook url provided by the user who has appropriate role. 




