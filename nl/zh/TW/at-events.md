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



# {{site.data.keyword.cloudaccesstrailshort}} 事件
{: #at_events}

您可以使用 {{site.data.keyword.cloudaccesstrailshort}} 服務，檢視、管理及審核在 {{site.data.keyword.security-advisor_long}} 服務實例中進行的使用者起始活動。
{: shortdesc}






如需服務運作方式的相關資訊，請參閱 [{{site.data.keyword.cloudaccesstrailshort}} 文件](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started#getting-started)。



## 檢視事件的位置
{: #monitor}

事件位在 {{site.data.keyword.cloudaccesstrailshort}} **帳戶網域**中，而此帳戶網域位於產生事件的 {{site.data.keyword.cloud_notm}} 地區。

1. 登入 {{site.data.keyword.cloud_notm}} 帳戶。
2. 從型錄中，在與 {{site.data.keyword.security-advisor_short}} 實例相同的帳戶中佈建 {{site.data.keyword.cloudaccesstrailshort}} 服務實例。
3. 在 {{site.data.keyword.cloudaccesstrailshort}} 儀表板的**管理**標籤上，按一下**在 Kibana 中檢視**。
4. 設定您要檢視其日誌的時間範圍。預設值為 15 分鐘。
5. 在**可用的欄位**清單中，按一下**類型**。按一下 **Activity Tracker** 的放大鏡圖示，將日誌限制為僅服務所追蹤的日誌。
6. 您可以使用其他可用的欄位，來縮小搜尋範圍。



## 事件清單
{: #events}

請參閱下表，以取得傳送至 {{site.data.keyword.cloudaccesstrailshort}} 的事件清單。
{: shortdesc}

<table>
  <tr>
    <th>動作</th>
    <th>說明</th>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>檢視一個以上先前建立的附註。</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>建立附註。</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>刪除附註。</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>更新附註。</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>檢視一個以上的出現項目。</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>建立出現項目。</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>刪除出現項目。</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>更新出現項目。</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>閱讀已新增至服務的自訂解決方案。</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>將自訂解決方案新增至服務。</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>更新服務內的現有自訂解決方案。</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>刪除服務中的自訂解決方案。</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>檢視已新增至服務實例的合作夥伴解決方案。</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>將合作夥伴解決方案新增至服務實例。</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>更新服務實例中的合作夥伴解決方案。</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>刪除服務實例中的合作夥伴解決方案。</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.enable</code></td>
    <td>啟用 {{site.data.keyword.security-advisor_short}} 所提供的 Network Insights 特性。</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.disable</code></td>
    <td>停用 {{site.data.keyword.security-advisor_short}} 所提供的 Network Insights 特性。</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.enable</code></td>
    <td>啟用 {{site.data.keyword.security-advisor_short}} 所提供的 Activity Insights 特性。</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.disable</code></td>
    <td>停用 {{site.data.keyword.security-advisor_short}} 所提供的 Activity Insights 特性。</td>
  </tr>
  <tr>
    <td><code>security-advisor.insights-cos.create</code></td>
    <td>透過 {{site.data.keyword.security-advisor_short}} 建立 Network  Insights 及 Activity Insights 的 Cloud Object Storage 實例。</td>
  </tr>
</table>
