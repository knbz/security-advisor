---

copyright:
  years: 2018
lastupdated: "2018-11-15"

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

{{site.data.keyword.security-advisor_short}} centralizes the insights services such as Vulnerability Advisor, and {{site.data.keyword.cloudcerts_short}} in {{site.data.keyword.Bluemix_notm}}. You can manage security events and apply analytics to create findings that unify and improve your security management processes on {{site.data.keyword.Bluemix_notm}}.

{{site.data.keyword.security-advisor_short}} also offers a trial network analytics capability that runs on your {{site.data.keyword.containerlong_notm}} cluster to gather net-flow data that can detect communication with suspicious clients and server IPs.

## Key concepts
{: #concepts}

Learn about different concepts that you might use while working with {{site.data.keyword.security-advisor_short}}.
{: shortdesc}

<dl>
  <dt>Finding</dt>
    <dd>A finding is a priority security issue that is created when raw events are processed. Findings are made up of the key pieces of information that are needed to identify the who, what, when, and where of the issue. As a security admin, you can use {{site.data.keyword.security-advisor_short}} findings to prioritize and react to detected situations.</dd>
  <dt>Key Performance Indicator (KPI)</dt>
    <dd>A Key Performance Indicator is triggered when a finding's value is out of bounds from the range of acceptable performance for specific security controls on services and workloads.</dd>
  <dt>Note</dt>
    <dd>You can create notes to categorize the findings that you come across while you are analyzing. A note can occur multiple times across different providers.</dd>
  <dt>Occurrence</dt>
    <dd>An occurrence describes provider-specific details of a note. The occurrence contains the vulnerability details, remediation steps, and other general information.</dd>
  <dt>Service CRN</dt>
    <dd>The Service CRN identifies the {{site.data.keyword.Bluemix_notm}} service that is involved in the finding.</br>
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
          <td>Network Analytics</td>
          <td>The Kubernetes cluster CRN.</td>
        </tr>
        <tr>
          <td>Vulnerability Advisor</td>
          <td>The container CRN.</td>
        </tr>
      </table></dd>
    <dt>Resource CRN</dt>
      <dd>The resource CRN identifies the specific resource that is involved in the finding.</dd>
</dl>
