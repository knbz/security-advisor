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

# Activity Insights
{: #setup-activity}

使用 {{site.data.keyword.security-advisor_long}}，您可以連續監視 {{site.data.keyword.cloud_notm}} Activity Tracker 日誌，以識別資源中未獲授權或可疑的行為及變更。您可以使用根據服務所提供最佳作法的規則套件，或建立您自己的自訂規則。
{: shortdesc}


## 開始之前
{: #activity-prereq}

若要開始使用 Activity Insights，請確定您具有下列必要條件。

- 如果您在 Windows 10 中工作，請啟動 Windows Subsystem for Linux，並安裝 [Ubuntu Shell](https://win10faq.com/install-run-ubuntu-bash-windows-10/){: external}。
- 安裝 yq CLI：
  * 針對 [macOS 及 Windows 10](http://mikefarah.github.io/yq/){: external}。
  * 針對 CentOS、Red Hat 及 Ubuntu，執行下列指令來安裝 1.15 版。
    ```
    wget https://github.com/mikefarah/yq/releases/download/1.15.0/yq_linux_amd64       
    mv yq_linux_amd64 yq   
    chmod +x yq    
    mv yq /usr/local/bin/     
    yq -V
    ```
    {: codeblock}     
- 已更新 cURL 二進位：針對 CentOS 及 Red Hat，您可以執行 `yum update -y nss curl libcurl` 進行更新。
- [{{site.data.keyword.cloud_notm}} CLI 及必要的外掛程式](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)
- [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/){: external} 1.10.11 版或更新版本
- [Kubernetes Helm（套件管理程式）](/docs/containers?topic=containers-helm)2.9.0 版或更新版本。
- 標準 Kubernetes 叢集 1.10.11 版或更新版本


## 建立 COS 儲存區
{: #activity-setup-cos}

使用 {{site.data.keyword.security-advisor_short}} GUI，即可建立新的 COS 實例及儲存區。

1. 導覽至服務儀表板的**整合**標籤。

2. 將 Activity Insights 方框中的**停用分析**切換為**啟用分析**。

3. 按一下**移至設定**。

4. 在「必要條件」區段中，按一下**建立 COS 實例及儲存區**。即會自動為您建立 COS 實例及儲存區，並且具有適當的命名慣例及 IAM 許可權。會顯示儲存區資訊。

如果您有現有的 COS 及儲存區實例，請確定它使用命名慣例 `sa.<account_id>.telemetric.<cos_region>`。若要容許服務讀取 COS 實例中所儲存的資料，請使用 {{site.data.keyword.cloud_notm}} IAM 來設定[服務對服務授權](/docs/iam?topic=iam-serviceauth)。將 `source` 設為 `{{site.data.keyword.security-advisor_short}}`，並將 `target` 設為 COS 實例。指派 `Reader` IAM 角色。


## 安裝 {{site.data.keyword.security-advisor_short}} 元件
{: #activity-install-components}

您可以安裝代理程式，以從 {{site.data.keyword.cloud_notm}} 帳戶收集審核流程日誌。日誌儲存在 Cloud Object Storage 實例中，而您可以在其中啟用 Activity Insights 來分析日誌中是否有可疑的行為。
{: shortdesc}

1. 將下列儲存庫複製至本端系統。

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. 切換至 `security-advisor-activity-insights` 資料夾。

3. 執行下列指令，以解壓縮 `.tar` 檔案。

  ```
  tar -xvf security-advisor-activity-insights.tar
  ```
  {: codeblock}

4. 切換至 `security-advisor-activity-insights` 資料夾。
5. 登入 {{site.data.keyword.cloud_notm}} CLI。遵循 CLI 中的提示，以完成登入。

  ```
  ibmcloud login -a cloud.ibm.com -r <region>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>地區</th>
      <th>端點</th>
    </tr>
    <tr>
      <td>英國</td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>美國南部</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

6. 設定叢集的環境定義。

  1. 讓指令設定環境變數，並下載 Kubernetes 配置檔。

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. 複製開頭為 `export` 的輸出，並將它貼到您的終端機以設定 `KUBECONFIG` 環境變數。

7. 使用 [Kubernetes Service 整合文件](/docs/containers?topic=containers-helm)來安裝 Helm。

8. 選用項目：[啟用 TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md){: external}。如果您使用工作站來處理多個叢集中的分析元件安裝，並啟用 TLS，則請確定 TLS 配置是最新的，並且符合您計劃要在其中安裝元件的現行叢集。

9. 執行下列指令，以安裝 Insights。此指令會驗證儲存區的命名慣例、建立 Kubernetes 密碼、使用叢集 GUID 更新這些值，以及部署 Activity Insights。

  ```
  ./activity-insight-install.sh <cos_region> <cos_api_key> <at_region> <account_api_key> <account_spaces>
  ```
  {: codeblock}

  如果您發生錯誤，請嘗試執行 `helm init --upgrade`。
  {: tip}

  <table>
    <tr>
      <th>變數</th>
      <th>說明</th>
    </tr>
    <tr>
      <td><code>cos_region</code></td>
      <td>COS 實例部署所在的地區。選項包括：<code>us-south</code> 及 <code>eu-gb</code>。</td>
    </tr>
    <tr>
      <td><code>cos_api_key</code></td>
      <td>您建立來存取 COS 實例及儲存區的 [API 金鑰](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials)。此金鑰必須具有平台角色 `writer`。</td>
    </tr>
    <tr>
      <td><code>at_region</code></td>
      <td>您在其中建立 COS 實例及儲存區的地區。選項包括：<code>us-south</code> 及 <code>eu-gb</code>。</td>
    </tr>
    <tr>
      <td><code>account_api_key</code></td>
      <td>{{site.data.keyword.cloud_notm}} 帳戶的平台 API 金鑰。</td>
    </tr>
    <tr>
      <td><code>account_spaces</code></td>
      <td>{{site.data.keyword.cloud_notm}} 帳戶的空間 GUID 清單（以逗點區隔）。</td>
    </tr>
  </table>


## 將規則套件新增至 COS
{: #activity-adding-rules}

規則套件是包含您要監視之規則清單的 JSON 檔案。您可以下載規則套件，或[建立您自己的規則套件](/docs/services/security-advisor?topic=security-advisor-activity#activity-packages)。{{site.data.keyword.security-advisor_short}} 引擎會驗證每個規則都遵循正確的語法。
{: shortdesc}

1. 複製下列儲存庫以取得數個預設規則套件。將在本端系統上建立名稱為 `security-advisor-activity-insights` 的資料夾。

  ```
  https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. 在本端建立名稱為 `IBM.rules/activities` 的資料夾。

3. 將 JSON 檔案從 `security-advisor-activity-insights/security-advisor-ata-rule-packages` 複製到 `IBM.rules/activities`。

4. 導覽至「{{site.data.keyword.cloud_notm}} 儀表板」，然後選取與 Activity Insights 相關聯的 COS 服務實例。

5. 在服務儀表板的**儲存區**標籤上，選取與 Activity Insights 相關聯的儲存區。

5. 在 COS 實例首頁上，按一下**上傳**，然後選取**資料夾**。

6. 如果系統出現提示，請按一下**安裝 Aspera Connect 用戶端**。如果您看不到提示，表示您已安裝用戶端。如果您需要安裝用戶端，則請在安裝完成時重複步驟 5。

7. 選取 *IBM.rules/activities* 資料夾。

8. 按一下**上傳**。

要使用您自己的套件嗎？使用其中一個 JSON 檔案作為指引，並建立符合組織需求的規則。建立檔案之後，請將它新增至 COS 實例中的 *IBM.rules/activities* 資料夾。如需規則類型及格式的相關資訊，請參閱[瞭解規則套件](/docs/services/security-advisor?topic=security-advisor-activity)。
{: tip}


## 刪除元件
{: #activity-delete}

如果您不再需要使用 Activity Insights，則可以從叢集中刪除服務元件。
{: shortdesc}

1. 使用 Helm 來刪除服務元件。如果已啟用 TLS，則請務必使用 `-tls` 旗標。

  ```
  helm del --purge activity-insights [--tls]
  ```
  {: codeblock}

2. 刪除 Kubernetes 密碼。

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}
