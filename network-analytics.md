---

copyright:
  years: 2018
lastupdated: "2018-07-27"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}

# Network Analytics
{: #network-analytics}

With {{site.data.keyword.Bluemix}} Security Advisor, you can gain insight into potentially hazardous network communication that is related to your {{site.data.keyword.containerlong_notm}} Kubernetes clusters. To preview this network analytics capability, select **Preview** from the Security Advisor [Getting Started page](https://console.bluemix.net/security/advisor/#!/overview).
{: shortdesc}

The network analytics preview feature consists of three modules:

1. **Net-flow collecting agent**: Installed on your cluster, the agent collects network information and sends it to the analytics backend. Read more about [data collection](#data-collection).

2. **Analytics backend**: The analytics backend identifies suspicious communication to and from the cluster. It uses threat intelligence that is offered by [IBM X-Force![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/security/xforce) to interpret the network information. When a suspicious communication is identified, the analytics backend monitors such communications and sends out security findings to the Security Advisor dashboard.

3. **Security Advisor dashboard**: Findings and KPIs from the analytics backend are presented in two cards:

   - **Suspicious server IPs card (outgoing traffic)**: This card shows KPIs and Findings regarding suspicious external IPs accessed by a client that is running as part of the cluster, such as when a workload approaches a server that is suspected to distribute malware. Learn more about [suspicious server IPs](#suspicious-server-ips).
   - **Suspicious Clients card (incoming traffic)**: This card shows KPIs and Findings regarding suspicious external clients that are accessing the cluster, such as when a member of a botnet approaches one of the cluster IPs. Learn more about [suspicious clients](#suspicious-clients).

## Data collection
{: #data-collection}

Security Advisor can help protect your cluster by adding network monitoring. Deployed as part of your cluster, network monitoring is used to provide information about cluster communications. To enable Network Analytics, metadata is collected about each network communication between the cluster and external entities.
{: shortdesc}

Information that is collected includes the IP addresses of the peers that are communicating, the ports that they use, the set of protocols that are being used, the amount of data that is transferred, a set of protocol-specific parameters, and various time stamps. **The actual data that is exchanged is not collected**.

Network monitoring works as part of the cluster with other services such as {{site.data.keyword.loganalysisshort_notm}} so that you can focus on the business logic. You can review the network monitoring insights in your Security Advisor dashboard.

## Suspicious server IPs (outgoing traffic)
{: #suspicious-server-ips}

The Security Advisor dashboard includes a suspicious server IP card that summarizes various information about communications in which the cluster acts as a client that approaches an external server.
{: shortdesc}

Analyzed communication might produce a finding such as:

- A client from within the cluster, such as workload deployment, approaches an external node with poor reputation, such as a botnet. Such communication might suggest that the workload is compromised and requires attention by your security team. The finding varies based on the reputation of the approached external node, the communication patterns, and the level of certainty offered by the intelligence.

- The cluster URL or one of its IPs has poor reputation. Review a poor reputation to check that the cluster is not compromised or otherwise engaged in unacceptable activity.

## Suspicious clients (incoming traffic)
{: #suspicious-clients}

The Security Advisor dashboard includes a suspicious client card that summarizes various information about communications in which the cluster acts as a server that is approached by an external client.
{: shortdesc}

Analyzed communication might produce a finding such as:

- An external node with poor reputation, such as a node that is known to distribute malware or is associated with anonymous services, approaches one of the cluster associated URLs or IPs. For example, a scanner approaches one of the cluster open ports. Such communication might suggest inadequate protection of cluster associated URLs and IPs, and also pending or actual hazards.
