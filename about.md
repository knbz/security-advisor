---

copyright:
  years: 2018
lastupdated: "2018-03-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# About Security Advisor
{: #about}

With {{site.data.keyword.Bluemix}} Security Advisor security admins can find, prioritize and manage security issues in their cloud applications and workloads.
{:shortdesc}

Security Advisor centralizes the insights of {{site.data.keyword.Bluemix_notm}} services such as Vulnerability Advisor, and {{site.data.keyword.cloudcerts_short}}. You can manage security events and apply analytics to create findings that unify and improve your security management processes on {{site.data.keyword.Bluemix_notm}}.

Security Advisor also offers a trial preview capability that runs on your {{site.data.keyword.containerlong_notm}} Kubernetes cluster to gather net-flow data that can detect communication with suspicious clients and server IPs.

## Security Advisor concepts
{: #concepts}

Learn about different concepts that you can use, track, and visualize in Security Advisor.

**Finding**  
A finding is a priority security issue that is created when raw events are processed. Findings are made up of the key pieces of information that are needed to identify the who, what, when and where. As a security admin, use Security Advisor reports of findings to prioritize and react to the detected situation.

**Key Performance Indicator (KPI)**  
A Key Performance Indicator is triggered when a finding's value is out of bounds from the range of acceptable performance for specific security controls on services and workloads.

**Service CRN**  
The Service CRN identifies the {{site.data.keyword.Bluemix_notm}} service involved in the finding.

- For {{site.data.keyword.cloudcerts_short}} findings, the service CRN is the Certificate Manager instance CRN.
- For Network Analytics findings, the service CRN is the Kubernetes cluster CRN.
- For Vulnerability Advisor findings, the service CRN is the Container CRN.

**Resource CRN**  
The resource CRN identifies the resource involved in the finding.
