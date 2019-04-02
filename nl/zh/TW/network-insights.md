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


# Network Insights（測試版）
{: #network}

使用 {{site.data.keyword.security-advisor_long}}，您可以根據威脅情報、行為型樣及機器學習，瞭解在您 {{site.data.keyword.containerlong_notm}} 叢集上執行的可能受損容器。
{: shortdesc}


## 如何運作
{: #network-how-it-works}

Network Insights 特性是 {{site.data.keyword.security-advisor_short}} 服務的附加程式。啟用及配置此特性時，會收集叢集與外部實體之間的叢集網路通訊（送入及送出），並對其持續監視及分析。

請查看下列影像，以查看資訊流程。

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

所收集的原始資料儲存至 Cloud Object Storage 儲存區，您可以在其中判斷其儲存時間長度。您擁有並控制所收集的資料，這表示您負責儲存、保護及刪除它。{{site.data.keyword.security-advisor_short}} 會維護發現項目 90 天。在這個期間，結果會呈現在服務儀表板的 Network Insights 卡上。因此，雖然您在 90 天之後就不會再於儀表板中看到發現項目，但在儲存空間中可能仍然有原始資料。

收集的資訊如下：

* 正在通訊之對等節點的 IP 位址
* 它們所使用的埠
* 正在使用的通訊協定集
* 已傳送的資料量
* 一組通訊協定特定參數
* 各種時間戳記。

**未收集所交換的實際資料。**

從安全觀點來看，在法律或公司需求容許刪除所收集的資料時，最好予以清除。如需相關資訊，請查看[刪除物件](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion)。
{: tip}


## 網路：可疑的入埠資料流量
{: #network-suspicious-inbound}

{{site.data.keyword.security-advisor_short}} 會在服務儀表板的**可疑的入埠資料流量**卡上通知您外部用戶端對調查及損害叢集所做的嘗試。
{: shortdesc}


會持續監視 IBM X-Force 分類為配送惡意軟體的用戶端行為型樣，而配送惡意軟體用來作為殭屍網路一部分的掃描器，以用於採礦加密貨幣或匿名化服務。如果該類型的用戶端接近您的叢集、顯示受監視叢集，以及顯示警示行為，則 Network Insights 會發出發現項目。


該卡片引進兩個「關鍵風險指標 (KRI)」：

* 可疑用戶端的偵察：包括與存取叢集的用戶端相關的發現項目。
* 可疑用戶端所傳送的異常大型有效負載：包括與用戶端與叢集之間傳送之資料磁區相關的發現項目。異常有效負載是任何超出您叢集之字元的項目。


發現項目可能包括可疑的用戶端，以：

* 將異常大量的資料傳送至叢集。
* 執行異常大量的叢集服務要求。
* 似乎將叢集設為目標，而叢集是透過執行以調查叢集的嘗試次數所顯示。



## 網路：可疑的出埠資料流量
{: #network-suspicious-outbound}

{{site.data.keyword.security-advisor_short}} 會在服務儀表板的**可疑的出埠資料流量**卡上通知您是否有任何在 {{site.data.keyword.containershort_notm}} 叢集上執行的可能受損容器。
{: shortdesc}

此服務會持續監視可存取 IBM X-Force 分類為配送惡意軟體之用戶端的容器行為型樣，而配送惡意軟體用來作為殭屍網路一部分的掃描器，以用於採礦加密貨幣或匿名化服務。在受監視叢集上的容器接近可疑的對等節點並顯示警示行為之後，Network Insights 會發出發現項目。

該卡片引進兩個「關鍵風險指標 (KRI)」：

* 可疑伺服器的出埠方式：與可存取伺服器的叢集相關的發現項目。
* 與可疑伺服器交換的大量有效負載：與叢集與伺服器之間傳送之資料磁區相關的發現項目。


發現項目可能包括容器，以：

* 將異常大量的資料傳送至可疑的伺服器
* 從可疑的伺服器擷取大量資料
* 執行大量的可疑伺服器要求


## 網路：資料流量異常（實驗性）
{: #network-anomalies}

在此實驗特性中，{{site.data.keyword.security-advisor_short}} 會監視您的網路以瞭解行為型樣。形成型樣之後，服務儀表板的**資料流量異常**卡中會標示並彙總所有似乎異常的行為。
{: shortdesc}

透過監視容器與其對等節點之間的行為型樣，來建立正常容器行為的模型。擷取模型時，會監視該模型是否有特定變更。如果顯示警示變更，則 Network Insights 會發出發現項目。

該卡片引進兩個「關鍵風險指標 (KRI)」：

* 高於正常偵察或資料交換活動：與叢集與任何外部對等節點之間偵測到異常互動相關的發現項目。
* 新伺服器的出埠方式：與叢集接近的新偵測到伺服器相關的發現項目。

發現項目可能包括：  

* 接近先前從未接近的伺服器的容器
* 送出或使用的資料比到達或來自特定對等節點的正常資料明顯還要多的容器
* 特定大量增加的調查層次

 開發模型之後，會偵測到來自所學習模型的偏差，並在顯示警示變更時，Network Insights 會將發現項目張貼到「安全顧問」儀表板。「網路：資料流量異常」卡中會彙總發現項目。該卡片引進兩個「關鍵風險指標 (KRI)」。「高於正常偵察或資料交換活動」KRI 會計算與叢集與外部對等節點之間偵測到異常互動相關的發現項目數，而「新伺服器的出埠方式」KRI 會計算與叢集接近的新偵測到伺服器相關的發現項目數。  

## 後續步驟
{: #network-next}

準備好要開始了嗎？請查看[啟用 Network Insights](/docs/services/security-advisor/setup-network.html)！
