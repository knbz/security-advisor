---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-13"

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


# 夥伴
{: #setup-partner}

「IBM 事業夥伴」整合是將協力廠商提供者的重要警示及發現項目帶入 {{site.data.keyword.security-advisor_long}} 儀表板。這些夥伴與 IBM 合作，協助您塑造、簡化並引導您進行整合體驗。
{: shortdesc}

## 開始之前
{: #partner-before}

開始整合夥伴之前，請確定您具有下列必要條件。

* 確定您有要整合之夥伴的帳戶。
* 確定您有產生夥伴服務整合 URL 的必要管理許可權。
* 確定您有 {{site.data.keyword.cloud_notm}} 的 IAM 管理存取權及 {{site.data.keyword.security-advisor_short}} 的管理員存取權。

## 整合精靈
{: #wizard}

身為 {{site.data.keyword.cloud_notm}} 及「夥伴」帳戶中具有管理權限的管理者，您可以使用整合精靈來快速啟動並執行。精靈有四個基本步驟：

* 建立信任，並關聯 {{site.data.keyword.cloud_notm}} 及夥伴帳戶
* 複製必要資訊，例如帳戶之間的認證及 URL
* 將夥伴發現項目 meta 資料安裝到 {{site.data.keyword.security-advisor_short}}
* 將發現項目從夥伴張貼到 {{site.data.keyword.security-advisor_short}} 來驗證配對


## 整合 NeuVector
{: #setup-neuvector}

使用 NeuVector，您可以偵測網路威脅及違規，以防止容器型應用程式的攻擊。透過監視，您可以藉由偵測容器及主機中的 root 專用權提升、埠掃描、反向 Shell 及可疑檔案系統活動，防止惡意探索及強行離開。
{: shortdesc}

若要將 NeuVector 與 {{site.data.keyword.security-advisor_short}} 整合，您可以使用下列步驟：

1. 導覽至**整合 > 夥伴整合**，然後從提供的選項中選取 **NeuVector**。
2. 連接帳戶。
  1. 提供連線的名稱。
  2. 提供 NeuVector 儀表板 URL。
  3. 提供 NeuVector 配置 URL。
    1. 登入 NeuVector 並導覽至設定。
    2. 按一下**產生 URL**。
    3. 複製 URL，並將它貼入 {{site.data.keyword.security-advisor_short}} 整合精靈。
  4. 按**下一步**。
3. 驗證您提供讓服務產生服務 ID 及 API 金鑰的許可權，然後按**下一步**來建立與服務的連線。
4. 按一下**完成**。
5. 移至服務儀表板，檢閱 NeuVector 已傳送至 {{site.data.keyword.containershort_notm}} 的測試發現項目。



## 整合 Twistlock
{: #setup-twistlock}

您可以停止在環境中部署有漏洞的映像檔來防止風險。透過自動化及自訂原則強制執行，Twistlock 可在應用程式生命週期的每個階段提供完整控制。
{: shortdesc}

當您配置夥伴連線時，儀表板中會建立兩張卡片，以彙總來自 Twistlock 的發現項目。

**Twistlock 運行環境**引進兩個「關鍵風險指標 (KRI)」：

* 突發事件及審核總計：與雲端原生工作負載上突發事件或審核相關的發現項目。
* 防火牆審核總計：與防火牆問題相關的發現項目。

**Twistlock 漏洞**引進一個 KRI：

* 具有新漏洞的映像檔總計：與容器映像檔中找到的漏洞相關的發現項目。

您可以在 Twistlock 文件中進一步瞭解公司。

### 配置 Twistlock
{: #configure-twistlock}

1. 在 {{site.data.keyword.security-advisor_short}} 儀表板中，導覽至**整合 > 夥伴整合**，然後從提供的選項中選取 **Twistlock**。

2. 按一下**是，將我的帳戶連接至 {{site.data.keyword.security-advisor_short}}**。

  如果您還沒有帳戶，請按一下**否，協助我建立帳戶 > 建立帳戶**。您可以填寫您的資訊，而 Twistlock 銷售團隊將會聯絡您開始使用。
  {: note}

3. 提供連線的名稱。請確定該名稱對您的帳戶是唯一的，而且只使用英數字元、空格或連字號。

4. 選用項目：如果您還沒有配置 URL，請按一下**產生登錄 URL** 來移至 Twistlock 文件，並瞭解如何建立 URL。請確定您的 Twistlock 記號隨附授權以存取文件。

5. 在 Twistlock 儀表板中，導覽至**管理 > 警示**標籤，然後按一下**新增設定檔**。

6. 從**提供者**下拉清單中選取 **{{site.data.keyword.security-advisor_short}}**。

7. 按一下**複製**以使用提供的配置 URL。

8. 回到 {{site.data.keyword.security-advisor_short}} 儀表板，將 URL 貼入**輸入 Twistlock 配置 URL** 方框。

9. 按一下**連接夥伴**。

### 驗證連線
{: #twistlock-verify}

1. 在 {{site.data.keyword.security-advisor_short}} 儀表板中，確認是否如預期顯示 Twistlock 卡。

2. 在 Twistlock 儀表板中，重新整理**警示**標籤。將會顯示 {{site.data.keyword.security-advisor_short}} 連線。請開啟連線。

3. 驗證已勾選您要收到通知的警示類型，然後按一下**驗證**以確定連線完成。
