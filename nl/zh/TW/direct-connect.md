---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

keywords: Centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

---

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


# 直接連線
{: #setup-custom-gui}

使用 {{site.data.keyword.security-advisor_short}}，您可以整合現有的自訂安全工具，不論是開放程式碼、自訂開發還是協力廠商服務均可。您可以將 URL 新增至整合清單，以建立從 {{site.data.keyword.security-advisor_short}} 到另一個工具的直接連線。藉由建立鏈結，您可以輕鬆監視您使用的所有工具。
{: shortdesc}


## 開始之前
{: #custom-before-gui}

您必須先有要整合之合作夥伴的帳戶，才能新增整合。

{{site.data.keyword.security-advisor_short}} 不會持續保存任何與合作夥伴服務相關的認證。企業使用者必須使用 SAML 向 {{site.data.keyword.cloud_notm}} 及事業夥伴進行鑑別。
{: note}

## 配置連線
{: #custom-configure-connection}

1. 登入安全工具，並取得唯一的 URL。

2. 使用主控台登入 {{site.data.keyword.cloud_notm}}。

3. 按一下**自訂整合 > 直接連線**。即會顯示畫面。

  1. 為解決方案提供名稱。名稱中只能使用英數字元、空格及橫線 (-)。

  2. 輸入解決方案的 URL，格式為：`www.<website>.<domain>`。

  3. 上傳代表該工具的圖示或影像。

  4. 按一下**連接**來完成配置。{{site.data.keyword.security-advisor_short}} 會建立整合所需的構件，例如服務 ID、API 金鑰、帳戶 ID 及 meta 資料。已指派 `writer` 角色。
