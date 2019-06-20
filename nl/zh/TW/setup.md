---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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

# 利用預先整合的服務
{: #setup-services}

{{site.data.keyword.security-advisor_short}} 隨附數個預先移入的卡片，可協助您監視安全風險和威脅。
{: shortdesc}

下列服務 {{site.data.keyword.security-advisor_short}} 會自動建立以下項目的卡片：

* {{site.data.keyword.registrylong_notm}}
* {{site.data.keyword.cloudcerts_long_notm}}

雖然您不需要執行任何動作來建立 {{site.data.keyword.security-advisor_short}} 與服務之間的連線，但必須具有已配置資訊的服務實例。


## 監視容器映像檔中的漏洞
{: #setup-images}

使用 {{site.data.keyword.registryshort_notm}}，您可以存取 Vulnerability Advisor，以持續掃描 {{site.data.keyword.registryshort_notm}} 實例中的映像檔是否有潛在的安全問題。如果發現問題，則您會收到警示，並且可以在 {{site.data.keyword.security-advisor_short}} 儀表板中檢視綜合性的報告。
{:shortdesc}

進一步瞭解 [{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started)。


### 開始之前
{: #setup-before}

開始使用登錄之前，請確定已安裝下列 CLI 及外掛程式：
* [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli)
* Container Registry 外掛程式。

  ```
ibmcloud plugin install container-registry
```
  {: codeblock}


### 建立名稱空間
{: #setup-create-namespace}

1. 使用 CLI 登入您的帳戶。

  ```
  ibmcloud login --sso
  ```
  {: codeblock}

2. 登入 {{site.data.keyword.registryshort_notm}}。

  ```
  ibmcloud cr login
  ```
  {: codeblock}

3. 選用項目：建立名稱空間。您一律可以使用現有項目。

  ```
  ibmcloud cr namespace-add
  ```
  {: codeblock}

3. 標記映像檔。

  ```
  docker tag <image>:<tag> <region>.icr.io/<namespace>/<image>:<tag>
  ```
  {: codeblock}

5. 推送映像檔。

  ```
  docker push <region>.icr.io/<namespace>/<image>:<tag>
  ```
  {: codeblock}


將映像檔推送至 {{site.data.keyword.registryshort_notm}} 名稱空間之後，任何所發現漏洞的相關資訊都會顯示在服務儀表板的**具有漏洞的映像檔**卡片中。您也可以往下探查至特定映像檔以找出相關資訊，例如所有已識別漏洞及配置問題的說明。


## 監視憑證
{: #setup-certificates}

您知道 {{site.data.keyword.cloudcerts_long_notm}} 可以協助監視及管理 SSL/TLS 憑證嗎？透過整合 {{site.data.keyword.cloudcerts_short}} 與 {{site.data.keyword.security-advisor_short}}，您可以提前取得關於憑證到期的警示，而這有助於防止服務或應用程式中斷。
{:shortdesc}

根據上傳至 {{site.data.keyword.cloudcerts_short}} 的憑證到期資料，發現項目會先出現在 {{site.data.keyword.security-advisor_short}} 儀表板中 90、60、10 及 1 天，憑證才會到期。此外，您也會收到關於到期憑證的每日通知。每日通知會從您的憑證到期之後的那一天開始。

若要觸發手動更新，您可以嘗試將一天內到期的憑證上傳至 {{site.data.keyword.cloudcerts_short}} 實例。匯入成功時，您可以看到「關鍵風險指標 (KRI)」，而且發現項目會顯示在 {{site.data.keyword.security-advisor_short}} 儀表板中。

您可以閱讀文件來進一步瞭解 [{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager?topic=certificate-manager-getting-started)。
{: tip}

### 建立憑證
{: #setup-create-cert}

若要建立在一天內到期的自簽憑證，您可以在終端機中執行下列 OpenSSL 指令。

```
openssl req -x509 -newkey rsa:2048 -keyout key.pem -subj "/CN=myservice.com" -out server.pem -days 1 -nodes
```
{: codeblock}


### 上傳憑證
{: #setup-upload-cert}

1. 在 {{site.data.keyword.cloud_notm}} 型錄中，搜尋 "{{site.data.keyword.cloudcerts_short}}"。
2. 提供服務實例的名稱，或使用預設名稱。
3. 按一下**建立**。
4. 若要將您組織的憑證匯入至 {{site.data.keyword.cloudcerts_short}}，請按一下**匯入憑證**。

既然您的憑證已匯入，{{site.data.keyword.security-advisor_short}} 儀表板的**憑證**卡片上就會顯示到期時間及過期憑證這類資訊。按一下該卡片，即可取得憑證的相關特定資訊，例如憑證所屬的服務實例。您也可以查看任何可採取來修正安全漏洞的步驟。

若要停止通知，您必須更新憑證、將憑證上傳至 {{site.data.keyword.cloudcerts_short}}，然後刪除過期憑證。
{: tip}
