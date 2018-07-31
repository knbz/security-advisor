---

copyright:
  years: 2018
lastupdated: "2018-07-31"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# About {{site.data.keyword.security-advisor_short}}
{: #about}

With {{site.data.keyword.security-advisor_long}} security admins can find, prioritize, and manage security issues in their cloud applications and workloads.
{:shortdesc}

{{site.data.keyword.security-advisor_short}} centralizes the insights of {{site.data.keyword.Bluemix_notm}} services such as Vulnerability Advisor, and {{site.data.keyword.cloudcerts_short}}. You can manage security events and apply analytics to create findings that unify and improve your security management processes on {{site.data.keyword.Bluemix_notm}}.

{{site.data.keyword.security-advisor_short}} also offers a trial preview capability that runs on your {{site.data.keyword.containerlong_notm}} cluster to gather net-flow data that can detect communication with suspicious clients and server IPs.

## Key concepts
{: #concepts}

Learn about different concepts that you might use while working with {{site.data.keyword.security-advisor_short}}.

**Finding**  
A finding is a priority security issue that is created when raw events are processed. Findings are made up of the key pieces of information that are needed to identify the who, what, when, and where. As a security admin, use {{site.data.keyword.security-advisor_short}} reports of findings to prioritize and react to the detected situation.

**Key Performance Indicator (KPI)**  
A Key Performance Indicator is triggered when a finding's value is out of bounds from the range of acceptable performance for specific security controls on services and workloads.

**Service CRN**  
The Service CRN identifies the {{site.data.keyword.Bluemix_notm}} service involved in the finding.

<table>
  <tr>
    <th>Type of finding</th>
    <th>CRN</th>
  </tr>
  <tr>
    <td>{{site.data.keyword.cloudcerts_short}}</td>
    <td>The service instance CRN.</td>
  </tr>
  <tr>
    <td>Network analytics</td>
    <td>The Kubernetes cluster CRN.</td>
  </tr>
  <tr>
    <td>Vulnerability Advisor</td>
    <td>The container CRN.</td>
  </tr>

**Resource CRN**  
The resource CRN identifies the resource involved in the finding.
