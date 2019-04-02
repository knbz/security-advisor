---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-14"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

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



# {{site.data.keyword.cloudaccesstrailshort}} 事件
{: #at_events}

您可以使用 {{site.data.keyword.cloudaccesstrailshort}} 服务来查看、管理和审计在 {{site.data.keyword.security-advisor_long}} 服务实例中执行的用户启动的活动。
{: shortdesc}

有关该服务如何工作的更多信息，请参阅 [{{site.data.keyword.cloudaccesstrailshort}} 文档](/docs/services/cloud-activity-tracker/reference?topic=cloud-activity-tracker-getting-started-with-cla#getting-started-with-cla)。


## 查看事件的位置
{: #monitor}

事件在生成事件的 {{site.data.keyword.Bluemix_notm}} 区域中可用的 {{site.data.keyword.cloudaccesstrailshort}} **帐户域**中提供。

1. 登录到 {{site.data.keyword.Bluemix_notm}} 帐户。
2. 在目录中，在 {{site.data.keyword.security-advisor_short}} 的实例所在的帐户中供应 {{site.data.keyword.cloudaccesstrailshort}} 服务的实例。
3. 在 {{site.data.keyword.cloudaccesstrailshort}} 仪表板的**管理**选项卡上，单击**在 Kibana 中查看**。
4. 设置要查看其日志的时间范围。缺省值为 15 分钟。
5. 在**可用字段**列表中，单击**类型**。单击 **Activity Tracker** 的“放大镜”图标，以将日志仅限于由该服务跟踪的日志。
6. 可以使用其他可用字段来缩小搜索范围。

要使帐户所有者以外的用户可查看日志，您必须使用高端套餐。要使其他用户可查看事件，请参阅[授予查看帐户事件的许可权](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions)。
{: tip}

## 事件列表
{: #events}

请查看下表以获取发送到 {{site.data.keyword.cloudaccesstrailshort}} 的事件的列表。
{: shortdesc}

<table>
  <tr>
    <th>操作</th>
    <th>描述</th>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>查看先前创建的注释。</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>创建注释。</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>删除注释。</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>更新注释。</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>查看一个或多个事件实例。</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>创建事件实例。</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>删除事件实例。</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>更新事件实例。</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>读取已添加到服务的定制解决方案。</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>向服务添加定制解决方案。</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>更新服务中的现有定制解决方案。</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>从服务中删除定制解决方案。</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>查看已添加到服务实例的合作伙伴解决方案。</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>向服务实例添加合作伙伴解决方案。</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>更新服务实例中的合作伙伴解决方案。</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>从服务实例中删除合作伙伴解决方案。</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.enable</code></td>
    <td>启用 {{site.data.keyword.security-advisor_short}} 提供的 Network Insights 功能。</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.disable</code></td>
    <td>禁用 {{site.data.keyword.security-advisor_short}} 提供的 Network Insights 功能。</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.enable</code></td>
    <td>启用 {{site.data.keyword.security-advisor_short}} 提供的 Activity Insights 功能。</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.disable</code></td>
    <td>禁用 {{site.data.keyword.security-advisor_short}} 提供的 Activity Insights 功能。</td>
  </tr>
  <tr>
    <td><code>security-advisor.insights-cos.create</code></td>
    <td>通过 {{site.data.keyword.security-advisor_short}} 创建 Cloud Object Storage 实例以用于 Network Insights 和 Activity Insights。</td>
  </tr>
</table>
