---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

---

{:new_window: target="_blank"}
{:external: target="_blank" .external}
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



# 管理服务访问权
{: #service-access}

作为帐户所有者，您可以使用 {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) 来管理对 {{site.data.keyword.security-advisor_long}} 实例的访问权。通过在帐户中设置策略，用于针对不同用户创建不同级别的访问权，可以确保 {{site.data.keyword.security-advisor_short}} 的每个实例都是安全的。
{: shortdesc}

有关 IAM 的更多信息，请参阅 [IAM 访问权](/docs/iam?topic=iam-userroles)。

## {{site.data.keyword.security-advisor_short}} 访问策略
{: #access}

帐户中访问 {{site.data.keyword.security-advisor_short}} 服务实例的每个用户都会分配有定义了 IAM 用户角色的访问策略。该策略确定用户可以在该特定服务实例的上下文中执行的操作。
{: shortdesc}

这些操作由 {{site.data.keyword.cloud_notm}} 服务定制和定义，作为允许在服务中执行的操作。然后，这些操作会映射到 IAM 服务用户角色。在下表中，{{site.data.keyword.security-advisor_short}} 的操作和必需的许可权一一对应。

<table><caption>哪些角色可以执行哪些操作？</caption>
  <col width="40%">
  <col width="40%">
  <col width="20%">
  <tr>
    <th>操作</th>
    <th>说明</th>
    <th>角色</th>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>查看安全报告发现结果。</td>
    <td>读取者</br>写入者</br>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>生成安全报告发现结果。</td>
    <td>写入者</br>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>删除安全报告发现结果。</td>
    <td>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>编辑安全报告发现结果。</td>
    <td>写入者</br>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>查看元数据。</td>
    <td>读取者</br>写入者</br>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>删除元数据。</td>
    <td>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>生成元数据。</td>
    <td>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>更新元数据。</td>
    <td>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>读取已添加到服务的定制解决方案。</td>
    <td>读取者</br>写入者</br>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>向服务添加定制解决方案。</td>
    <td>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>更新服务中的现有定制解决方案。</td>
    <td>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>从服务中删除定制解决方案。</td>
    <td>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>查看已添加到服务实例的合作伙伴解决方案。</td>
    <td>读取者</br>写入者</br>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>向服务实例添加合作伙伴解决方案。</td>
    <td>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>更新服务实例中的合作伙伴解决方案。</td>
    <td>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>从服务实例中删除合作伙伴解决方案。</td>
    <td>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.enable</code></td>
    <td>启用服务提供的 Network Insights。</td>
    <td>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.disable</code></td>
    <td>禁用服务提供的 Network Insights。</td>
    <td>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.enable</code></td>
    <td>启用服务提供的 Activity Insights。</td>
    <td>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.disable</code></td>
    <td>禁用服务提供的 Activity Insights。</td>
    <td>管理者</td>
  </tr>
  <tr>
    <td><code>security-advisor.insights-cos.create</code></td>
    <td>通过 {{site.data.keyword.security-advisor_short}} 创建 Cloud Object Storage 实例以用于 Network Insights 和 Activity Insights。</td>
    <td>管理者</td>
  </tr>
</table>

## API 映射
{: #api-map}

这些角色是如何对应于 API 的？请查看下表，了解如何将服务操作映射到 API。


| 方法   | 端点                                                                      | 服务操作                         |
|--------|---------------------------------------------------------------------------|----------------------------------|
| POST   | /v1/{account_id}/graph                                                    | security-advisor.findings.read   |
| POST   | /v1/{account_id}/providers/{provider_id}/notes                            | security-advisor.metadata.write  |
| GET    | /v1/{account_id}/providers/{provider_id}/notes                            | security-advisor.metadata.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.read   |
| PUT    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.update |
| DELETE | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.delete |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}/note | security-advisor.findings.read   |
| POST   | /v1/{account_id}/providers/{provider_id}/occurrences                      | security-advisor.findings.write  |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences                      | security-advisor.findings.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}/occurrences      | security-advisor.findings.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.read   |
| PUT    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.update |
| DELETE | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.delete |
