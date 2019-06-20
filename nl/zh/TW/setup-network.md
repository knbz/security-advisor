---

copyright:
  years: 2017, 2019
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

# Network Insights
{: #setup-network}

使用 {{site.data.keyword.security-advisor_long}}，您可以使用機器學習、學習的模式及威脅情報來監視行為，以偵測在您 {{site.data.keyword.containerlong_notm}} 叢集上執行的可能已遭盜用容器。
{: shortdesc}

從 2019 年 1 月 20 日開始，Network Insights（測試版）會取代 Network Analytics 特性。會刪除服務儀表板中的任何分析卡片，但發現項目會保留在發現項目資料庫中。
{: important}

## 開始之前
{: #network-prereq}

如果您目前使用 Network Analytics 特性，則必須先[刪除服務元件](/docs/services/security-advisor?topic=security-advisor-setup-network#uninstall-analytics)，才能安裝 Network Insights。若要開始使用 Network Insights，請確定您具有下列必要條件。

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
{: #network-setup-cos}

使用 {{site.data.keyword.security-advisor_short}} GUI，即可建立新的 COS 實例及儲存區。

1. 在服務儀表板的**整合**標籤上，將 Network Insights 方框中的**停用分析**切換為**啟用分析**。

2. 按一下**移至設定**。

3. 在「必要條件」區段中，按一下**建立 COS 實例及儲存區**。即會自動為您建立 COS 實例及儲存區，並且具有適當的命名慣例及 IAM 許可權。

如果您有現有的 COS 及儲存區實例，請確定它使用命名慣例：`sa.<account_id>.telemetric.<cos_region>`。若要容許服務讀取 COS 實例中所儲存的資料，請使用 {{site.data.keyword.cloud_notm}} IAM 來設定[服務對服務授權](/docs/iam?topic=iam-serviceauth)。將 `source` 設為 `{{site.data.keyword.security-advisor_short}}`，並將 `target` 設為 COS 實例。指派 `Reader` IAM 角色。


## 安裝 {{site.data.keyword.security-advisor_short}} 元件
{: #network-install-components}

您可以安裝代理程式，以從 Kubernetes 叢集中收集網路流程日誌。日誌儲存在 Cloud Object Storage 實例中。您接著可以啟用 Network Insights 來分析日誌，以及偵測並警告您是否有可疑的網路活動。
{: shortdesc}

請務必對要監視的每個叢集重複此安裝。
{: note}

1. 登入 {{site.data.keyword.cloud_notm}} CLI。遵循 CLI 中的提示，以完成登入。

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

2. 設定叢集的環境定義。

  1. 讓指令設定環境變數，並下載 Kubernetes 配置檔。

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. 複製開頭為 `export` 的輸出，並將它貼到您的終端機以設定 `KUBECONFIG` 環境變數。

3. 取得 Kubernetes 叢集的版本。

  ```
  kube_version=$(kubectl version --output json) echo $(echo $kube_version | yq r - serverVersion.major).$(echo $kube_version | yq r - serverVersion.minor)
  ```
  {: codeblock}

3. 如果使用的是 Kubernetes Service V1.12.x，請使用下列指令安裝 Helm。如果使用的是其他版本，請參閱 Kubernetes 文件，瞭解應該執行哪些安裝步驟[在具有公用存取權的叢集中設定 Helm](/docs/containers?topic=containers-helm#public_helm_install)。

  1. 刪除任何現有部署。

    ```
    kubectl delete deployment tiller-deploy -n kube-system
    ```
    {: codeblock}

  2. 將 Tiller RBAC 原則套用於部署。

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: codeblock}
  
  3. 在叢集中起始設定 Helm，然後在叢集中安裝 Tiller。

    ```
    helm init --service-account tiller
    ```
    {: codeblock}

  4. 藉由驗證 `tiller-deploy` Pod 的狀態是否為 `running`，檢查安裝是否成功。

    ```
    kubectl get pods -n kube-system -l app=helm
    ```
    {: codeblock}

4. 確保 Kubernetes Service 的版本為 1.11 或更高版本，然後將下列儲存庫複製到本端系統。

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-network-insights.git
  ```
  {: codeblock}

  Kubernetes Service V1.10 已淘汰使用且不受支援。如果已安裝 V1.10 或更高版本，請執行下列 Helm 指令重新啟動分析器 Pod 來修正現有映像檔中的漏洞：`helm upgrade --recreate-pods network-insights`。
  {: deprecated}

5. 切換至 `security-advisor-network-insights` 資料夾。

6. 切換至 `v1.10+` 目錄。

7. 執行下列指令，以解壓縮 `.tar` 檔案。

  ```
  tar -xvf security-advisor-network-insights.tar
  ```
  {: codeblock}

8. 切換至 `security-advisor-network-insights` 資料夾。

9. 使用 [Kubernetes Service 整合文件](/docs/containers?topic=containers-helm)來安裝 Helm。

10. 選用項目：[啟用 TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md){: external}。如果您使用工作站來處理多個叢集中的分析元件安裝，並啟用 TLS，則請確定 TLS 配置是最新的，並且符合您計劃要在其中安裝元件的現行叢集。

11. 執行下列指令，以安裝 Helm 圖表及其相依關係。此指令會驗證您的儲存區是否使用正確的命名慣例、建立 Kubernetes 密碼、使用叢集 GUID 更新這些值，以及部署 Network Insights Helm 圖表。如果您發生錯誤，請嘗試執行 `helm init --upgrade`。

  ```
  ./network-insight-install.sh <cos_region> <cos_api_key>
  ```
  {: codeblock}

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
  </table>

您已順利配置叢集來使用 {{site.data.keyword.security-advisor_short}} Network Insights！



## 刪除元件
{: #network-delete}

如果您不再需要使用 Network Insights，則可以從叢集中刪除服務元件。
{: shortdesc}

1. 登入 {{site.data.keyword.cloud_notm}} CLI。遵循 CLI 中的提示，以完成登入。

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

2. 設定叢集的環境定義。

  1. 讓指令設定環境變數，並下載 Kubernetes 配置檔。

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. 複製開頭為 `export` 的輸出，並將它貼到您的終端機以設定 `KUBECONFIG` 環境變數。

3. 使用 Helm 來刪除元件。

  ```
  helm del --purge network-insights [--tls]
  ```
  {: codeblock}

4. 刪除 Kubernetes 密碼。

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}

請務必對您要從中移除代理程式的每個叢集刪除該處理程序。
{: tip}

## 解除安裝 Network Analytics
{: #uninstall-analytics}

如果您已使用 Network Analytics 的測試版，則必須先解除安裝舊的 {{site.data.keyword.security-advisor_short}} 元件，才能安裝新的版本。請務必對包含任何服務元件的每個叢集重複此處理程序。
{: shortdesc}

1. 登入 {{site.data.keyword.cloud_notm}}。

  ```
  ibmcloud login -a https://api.us-south.ibm.cloud.com --sso
  ```
  {: codeblock}

2. 列出帳戶中的所有叢集，以取得叢集的名稱。

  ```
  ibmcloud ks clusters
  ```
  {: codeblock}

3. 設定叢集的環境定義。

  1. 讓指令設定環境變數，並下載 Kubernetes 配置檔。

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. 複製開頭為 `export` 的輸出，並將它貼到您的終端機以設定 `KUBECONFIG` 環境變數。

4. 導覽至解壓縮的保存檔位置，並執行解除安裝程式 Script。

  ```
  ./uninstall.sh
  ```
  {: codeblock}

5. 選用項目：從叢集中解除安裝 Helm 伺服器元件。

  ```
  helm reset
  ```
  {: codeblock}
