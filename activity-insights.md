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

## Next steps
{: #next}

Ready to get started? Create your own or download one of the provided [rule packages](rules.html).
