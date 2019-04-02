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


# 整合
{: #integrations}

使用 {{site.data.keyword.security-advisor_long}}，您可以整合其他內建見解、夥伴解決方案，或建立您自己的自訂整合。
{: shortdesc}


## 前置整合的發現項目
{: #integrate-built-in}

使用 {{site.data.keyword.security-advisor_long}}，您可以透過內建服務功能來深入瞭解潛在的問題。
{: shortdesc}


依預設，{{site.data.keyword.security-advisor_short}} 會與下列項目整合：

* Certificate Manager，用於與憑證期限及生命週期相關的通知。
* Vulnerability Advisor，用於取得映像檔漏洞及配置問題的詳細資料。

若要進一步瞭解，請查看[利用預先整合的服務](/docs/services/security-advisor?topic=security-advisor-setup-services)！

</br>

## 夥伴整合
{: #integrate-partner}

夥伴整合是加強 {{site.data.keyword.cloud_notm}} 部署安全的方法，方法是在 {{site.data.keyword.security-advisor_short}} 與 IBM 合作的其他安全工具之間建立連線，確保無縫式使用者體驗。
{: shortdesc}

現行 {{site.data.keyword.security-advisor_short}} 夥伴包括 Neuvector 及 Twistlock。

您是夥伴且有興趣整合解決方案與 {{site.data.keyword.security-advisor_short}} 嗎？請透過 wardm@us.ibm.com 聯絡 Matt Ward 來聯繫我們的團隊！
{: tip}

### NeuVector
{: #integrate-neuvector}

[NeuVector](https://neuvector.com/) 為 Kubernetes 及 Red Hat OpenShift 提供高度整合的自動化安全平台，可讓您監視及保護容器網路資料流量。明確地說是東西向網路資料流量。

使用 NeuVector，您可以偵測網路威脅及違規，以防止容器型應用程式的攻擊。透過監視，您可以藉由偵測容器及主機中的 root 專用權提升、埠掃描、反向 Shell 及可疑檔案系統活動，防止惡意探索及強行離開。

如需設定 NeuVector 實例的協助，請參閱[夥伴解決方案](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-neuvector)。


### Twistlock
{: #integrate-twistlock}

Twistlock 可以獨一無二地防止在整個環境中部署有漏洞的映像檔，來防止整個 SDLC 的風險。自動化及自訂原則強制執行可在應用程式生命週期的每個階段提供完整控制。Twistlock Intelligence Stream 會直接從 30+ 個來自 Twistlock Labs 的上游專案、商業來源及所有權研究獲得及聚集漏洞資訊。

聚焦於讓最精確的資料可涵蓋堆疊的所有層，讓您具有精確的可見性及最低的誤判率。Twistlock 會結合此資料與實際部署的知識，例如，向網際網路公開的容器、以高專用權執行的容器，以及已有其他安全降低的容器，讓您隨時都能夠查看特定環境中的最重要漏洞。

如需設定 Twistlock 實例的協助，請參閱[夥伴解決方案](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-twistlock)。
</br>


## 自訂整合
{: #integrate-custom}

您可能已經有一個相依的安全工具。您可以將該工具與 {{site.data.keyword.security-advisor_short}} 儀表板整合，以將所有安全資訊集中在一個位置！
{: shortdesc}

### 建立直接連線
{: #create-direct-connect}

使用 {{site.data.keyword.security-advisor_short}} GUI，即可將內部及外部安全工具加入書籤，讓您擁有從 {{site.data.keyword.security-advisor_short}} 儀表板中存取所有項目的一個存取點。例如，您可以將 PagerDuty 加入書籤。

### 整合您自己的工具與 API
{: #integrate-tools-api}

使用 Findings API，您可以將自訂安全工具中的發現項目整合到 {{site.data.keyword.security-advisor_short}} 儀表板。這可以是任何自訂或夥伴工具，它們會產生也支援 API 型互動的安全警示或發現項目。

## Built-in Insights
{: #integrate-insights}

使用 Built-in Insights，您可以透過持續監視叢集及帳戶日誌來偵測潛在問題。透過監視網路資料流量及使用者活動，您可以協助確保 {{site.data.keyword.cloud_notm}} 資源仍然受到保護。
{: shortdesc}

**Network Insights（測試版）**

使用 Network Insights（測試版），您可以監視及分析 Kubernetes 叢集與外部實體之間之間的叢集網路通訊（送入及送出）。透過使用整合式威脅情報及異常偵測，服務可以識別偵察攻擊以及可能已受損的資產。若要進一步瞭解，請查看 [Network Insights](/docs/services/security-advisor?topic=security-advisor-network)。

**Activity Insights（預覽）**

使用 Activity Insights（預覽），您可以持續監視 {{site.data.keyword.cloud_notm}} Activity Tracker 日誌，以識別使用者或應用程式利用規則套件所進行的未獲授權或可疑活動。您可以使用服務所提供並以安全最佳作法為基礎的規則套件，也可以自訂規則以符合您的需求。若要進一步瞭解，請查看 [Activity Insights](/docs/services/security-advisor?topic=security-advisor-activity)。
