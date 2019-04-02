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



# 管理服務存取
{: #service-access}

身為帳戶擁有者，您可以使用 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) 來管理對 {{site.data.keyword.security-advisor_long}} 實例的存取權。透過在帳戶內設定原則以針對不同的使用者建立不同的存取層次，您可以確保每個 {{site.data.keyword.security-advisor_short}} 實例都是安全的。
{: shortdesc}

如需 IAM 的相關資訊，請參閱 [IAM 存取](/docs/iam?topic=iam-userroles)。

## {{site.data.keyword.security-advisor_short}} 存取原則
{: #access}

帳戶中存取 {{site.data.keyword.security-advisor_short}} 服務實例的每位使用者，都必須獲指派已定義 IAM 使用者角色的存取原則。該原則決定使用者可在該特定服務實例的環境定義內執行的動作。
{: shortdesc}

這些動作是自訂的，並由 {{site.data.keyword.Bluemix_notm}} 服務定義為可在服務中執行的作業。然後，這些動作會對映至 IAM 服務使用者角色。在下表中，會對映 {{site.data.keyword.security-advisor_short}} 的動作與必要許可權。

<table><caption>哪些角色可以執行哪些動作？</caption>
  <col width="40%">
  <col width="40%">
  <col width="20%">
  <tr>
    <th>動作</th>
    <th>說明</th>
    <th>角色</th>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>檢視安全報告發現項目。</td>
    <td>讀者</br>作者</br>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>產生安全報告發現項目。</td>
    <td>作者</br>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>刪除安全報告發現項目。</td>
    <td>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>編輯安全報告發現項目。</td>
    <td>作者</br>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>檢視 meta 資料。</td>
    <td>讀者</br>作者</br>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>刪除 meta 資料。</td>
    <td>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>產生 meta 資料。</td>
    <td>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>更新 meta 資料。</td>
    <td>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>閱讀已新增至服務的自訂解決方案。</td>
    <td>讀者</br>作者</br>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>將自訂解決方案新增至服務。</td>
    <td>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>更新服務內的現有自訂解決方案。</td>
    <td>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>刪除服務中的自訂解決方案。</td>
    <td>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>檢視已新增至服務實例的夥伴解決方案。</td>
    <td>讀者</br>作者</br>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>將夥伴解決方案新增至服務實例。</td>
    <td>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>更新服務實例中的夥伴解決方案。</td>
    <td>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>刪除服務實例中的夥伴解決方案。</td>
    <td>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.enable</code></td>
    <td>啟用服務所提供的 Network Insights。</td>
    <td>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.disable</code></td>
    <td>停用服務所提供的 Network Insights。</td>
    <td>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.enable</code></td>
    <td>啟用服務所提供的 Activity Insights。</td>
    <td>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.disable</code></td>
    <td>停用服務所提供的 Activity Insights。</td>
    <td>管理員</td>
  </tr>
  <tr>
    <td><code>security-advisor.insights-cos.create</code></td>
    <td>透過 {{site.data.keyword.security-advisor_short}} 建立 Network 及 Activity Insights 的 Cloud Object Storage 實例。</td>
    <td>管理員</td>
  </tr>
</table>

## API 對映
{: #api-map}

這些角色如何對應至 API？請查看下表，以查看服務動作如何對映至 API。


| 方法   | 端點                                                                      |  服務動作                        |
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
