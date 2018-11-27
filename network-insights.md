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

# Network insights
{: #network}

With {{site.data.keyword.security-advisor_long}}, you can gain insight into potentially compromised containers that run on your {{site.data.keyword.containerlong_notm}} clusters. You can gain insights based on threat intelligence, behavioral patterns, and machine learning.
{: shortdesc}

The network insights feature is an add-on to the {{site.data.keyword.security-advisor_short}} service. With the feature enabled and configured, cluster network communication, both incoming and outgoing, is collected and continuously monitored and analyzed.

**How does it work?**

Network insights rely on the collection of network data through network collectors that are offered by {{site.data.keyword.security-advisor_short}}. The data is collected and stored in a Cloud Object Storage bucket where the service read and analyze it. Don't worry, you own and control the collected data. The results are then presented on the network insights cards in the {{site.data.keyword.security-advisor_short}} dashboard.

The dashboard provides network insights via three cards:

* Reconnaissance attempts
* Possible compromised assets
* Abnormal network behavior


**How can I use network insights?**

To get started with network insights, you must be an account administrator. If that's you, follow these steps:

1. Navigate to the {{site.data.keyword.security-advisor_short}} dashboard.
2. Create an Object Storage bucket.
3. Enable the Network insights add-on.
4. Follow the set up wizard for help installing the Netflow Collectors for each cluster that you want to monitor.

When the installation is complete, you will have an Object Storage bucket. Network insights will have the authorization to read and analyze the collected data and then provide you with findings on your {{site.data.keyword.security-advisor_short}} dashboard.


</br>


## Collecting data
{: #data}


{{site.data.keyword.security-advisor_short}} can help protect your cluster by adding network monitoring.
{: shortdesc}

Deployed as part of your cluster, network monitoring is used to provide information about cluster communications. To enable insights, network flow information is collected about the communication that takes place between the cluster and external entities.


The following information is collected:

* The IP address of the peers that are communicating
* The ports that they use
* The set of protocols that are being used
* The amount of data that is transferred
* A set of protocol-specific parameters
* Various time stamps.

The actual data that is exchanged is not collected.
{: tip}

All of the network data that is collected is stored within an Object Storage bucket. As the account admin, you might access this data for forensics or to integrate additional analytics tools.

Network monitoring works as part of the cluster with other services such as {{site.data.keyword.loganalysisshort_notm}} so that you can focus on the business logic. You can review the network monitoring insights in your {{site.data.keyword.security-advisor_short}} dashboard.

</br>

## Reconnaissance attempts
{: #recon-attempts}

{{site.data.keyword.security-advisor_short}} informs you of attempts that are made by external peers to survey and compromise clusters on a reconnaissance attempts card.
{: shortdesc}

The behavioral patterns of peers that are classified by IBM X-Force as distributing malware that is used as scanners, as part of a botnet, for mining cryptocurrency, or for anonymization services are all continuously monitored. When these peers approach a monitored cluster and exhibit an alarming behavior, network insights issues a finding.

Analyzed communication findings might include suspicious peers that:

* send abnormally large amounts of data into the cluster
* perform an abnormally large number of requests to a cluster service
* appear to target the cluster as exhibited by the number of attempts that it performs to survey the cluster and identity what services it has open in any given time

</br>

## Possible compromised assets
{: #compromised}

The {{site.data.keyword.security-advisor_short}} dashboard includes a **Possible Compromised Assets** card that summarizes any indication of potentially compromised containers that run on your {{site.data.keyword.containershort_notm}} clusters.
{: shortdesc}

The behavioral patterns of containers that access peers that are classified by IBM X-Force as distributing malware that is used as scanners, as part of a botnet, for mining cryptocurrency, or for anonymization services are all continuously monitored. After a container on a monitored cluster approaches the suspicious peers and exhibits alarming behavior, network insights issues a finding.

Analyzed communication findings might include containers that:

* send abnormally large amounts of data to a suspicious peer
* extract a large amount of data from a suspicious peer
* perform a large number of requests to a suspicious peer

</br>

## Abnormal network behavior
{: #behavior}

The {{site.data.keyword.security-advisor_short}} dashboard includes a **Abnormal Network Behavior** card that summarizes any abnormal behavior that is displayed by containers that run on your {{site.data.keyword.containershort_notm}} clusters.
{: shortdesc}

The behavioral patterns between containers and peers are learned over time and a model of the normal behavior is captured. After the model is captured, specific changes from the learned model are monitored. When an alarming change is exhibited, network insights issues a finding.  

Analyzed communication findings might include:

* containers that approach peers that they haven't previously communicated with
* containers that send out or consume significantly more data than they normally do to or from specific peers.
* the level of surveying experienced by a cluster has significantly increased
