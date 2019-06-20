---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-06"

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


# Network Insights（測試版）
{: #network}

使用 {{site.data.keyword.security-advisor_long}}，您可以根據威脅情報、行為模式及機器學習，瞭解在您 {{site.data.keyword.containerlong_notm}} 叢集上執行的可能已遭盜用容器。
{: shortdesc}


## 如何運作
{: #network-how-it-works}

Network Insights 特性是 {{site.data.keyword.security-advisor_short}} 服務的附加程式。啟用及配置此特性時，會收集叢集與外部實體之間的叢集網路通訊（送入及送出），並對其持續監視及分析。

請參閱下列影像，以瞭解資訊流程。

![Network Insights 流程圖](images/network-insights-flow.png)

1. 身為帳戶管理者，您可以將 Network Insights 安裝至要監視的每個叢集。
2. 除非您決定刪除網路流程日誌，否則會將網路流程日誌轉遞至其儲存所在的 Cloud Object Storage 儲存區。當您使用 {{site.data.keyword.security-advisor_short}} GUI 來建立儲存區時，會指派服務對服務 IAM 角色，讓服務可以檢視日誌。
3. 啟用 Network Insights 時，會分析 COS 儲存區中的原始資料是否有可疑的行為。
4. 標示可能的安全問題時，會將發現項目轉遞至「發現項目」資料庫。
5. 發現項目會顯示在服務儀表板的三張卡片上。
  * 可疑的入埠資料流量
  * 可疑的出埠資料流量
  * 資料流量異常


## 收集資料
{: #network-data}

{{site.data.keyword.security-advisor_short}} 提供收集器，用來收集叢集與外部實體之間發生的網路流程資訊。
{: shortdesc}

所收集的原始資料儲存在 Cloud Object Storage 儲存區，您可以在其中確定其儲存時間長度。您擁有並控制所收集的資料，這表示您負責儲存、保護及刪除它。{{site.data.keyword.security-advisor_short}} 會保留發現項目 90 天。在這個期間，結果會呈現在服務儀表板的 Network Insights 卡片上。因此，雖然您在 90 天之後就不會再於儀表板中看到發現項目，但在儲存空間中可能仍然有原始資料。

收集的資訊如下：

* 正在通訊之對等節點的 IP 位址
* 它們所使用的埠
* 正在使用的通訊協定集
* 已傳送的資料量
* 一組通訊協定特定參數
* 各種時間戳記。

不會收集已交換的實際資料。
{: tip}

從安全觀點來看，在法律或公司需求容許刪除所收集的資料時，最好予以清除。如需相關資訊，請參閱[刪除物件](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion)。



## 網路：可疑的入埠資料流量
{: #network-suspicious-inbound}

{{site.data.keyword.security-advisor_short}} 會在服務儀表板的**可疑的入埠資料流量**卡片上通知您外部用戶端對調查及損害叢集所做的嘗試。
{: shortdesc}


對於 IBM X-Force 分類為散佈用作掃描器、用作僵屍網路一部分、用於挖掘加密貨幣或用於匿名化服務之惡意軟體的用戶端，系統將持續監視所有這些用戶端的行為模式。如果該類型的用戶端接近受監視的叢集，並表現出令人警惕的行為，Network Insights 會發出發現項目。


該卡片引進兩個「關鍵風險指標 (KRI)」：

* 可疑用戶端的偵察：包括與存取叢集的用戶端相關的發現項目。
* 可疑用戶端所傳送的異常大型有效負載：包括與用戶端與叢集之間傳送之資料磁區相關的發現項目。異常有效負載是任何超出您叢集之字元的項目。


發現項目可能包括可疑的用戶端，其：

* 將異常大量的資料傳送至叢集。
* 執行異常大量的叢集服務要求。
* 似乎鎖定叢集作為目標，並顯示試圖執行來調查叢集的嘗試次數。



## 網路：可疑的出埠資料流量
{: #network-suspicious-outbound}

{{site.data.keyword.security-advisor_short}} 會在服務儀表板的**可疑的出埠資料流量**卡片上通知您是否有任何在 {{site.data.keyword.containershort_notm}} 叢集上執行的可能已遭盜用容器。
{: shortdesc}

此服務會持續監視可存取 IBM X-Force 分類為散佈惡意軟體之用戶端的容器行為模式，而散佈惡意軟體用來作為殭屍網路一部分的掃描器，以用於採礦加密貨幣或匿名化服務。在受監視叢集上的容器接近可疑的對等節點並顯示警示行為之後，Network Insights 會發出發現項目。

該卡片引進兩個「關鍵風險指標 (KRI)」：

* 出埠接近可疑伺服器：與存取伺服器的叢集相關的發現項目。
* 與可疑伺服器交換的異常大量有效負載：與叢集與伺服器之間傳送之資料磁區相關的發現項目。


發現項目可能包含有下列行為的容器：

* 將異常大量的資料傳送到可疑伺服器。
* 從可疑的伺服器擷取大量資料。
* 對可疑伺服器執行大量要求。


## 網路：資料流量異常（實驗性）
{: #network-anomalies}

在此實驗特性中，{{site.data.keyword.security-advisor_short}} 會監視您的網路以瞭解行為模式。形成模式之後，服務儀表板的**資料流量異常**卡片中會標示並彙總所有似乎異常的行為。
{: shortdesc}

透過監視容器與其對等節點之間的行為模式，來建立正常容器行為的模型。擷取模型時，會監視該模型是否有特定變更。如果顯示警示變更，則 Network Insights 會發出發現項目。

該卡片引進兩個「關鍵風險指標 (KRI)」：

* 高於正常偵察或資料交換活動：與叢集與任何外部對等節點之間偵測到異常互動相關的發現項目。
* 出埠接近新伺服器：與叢集接近的新偵測到伺服器相關的發現項目。

發現項目可能包括：  

* 接近先前從未接近之伺服器的容器。
* 送出或使用的資料比到達或來自特定對等節點的正常資料明顯還要多的容器。
* 對特定容器的調查層次大幅增加。

開發模型之後，會偵測到來自所學習模型的偏差，並在顯示警示變更時，Network Insights 會將發現項目張貼到 Security Advisor 儀表板。發現項目在「網路：資料流量中的異常」卡片中進行彙總。該卡片引進兩個「關鍵風險指標 (KRI)」。「高於正常的偵察或資料交換活動」KRI 對與在叢集和外部同層級之間偵測到的異常互動相關的發現項目計數。「對新伺服器的出埠接近」KRI 會計算與叢集之新偵測到伺服器接近相關的發現項目。  

## 後續步驟
{: #network-next}

準備好要開始了嗎？請參閱[啟用 Network Insights](/docs/services/security-advisor?topic=security-advisor-setup-network)！
