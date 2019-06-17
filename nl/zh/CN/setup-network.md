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

通过 {{site.data.keyword.security-advisor_long}}，您可以使用机器学习、学习到的模式和威胁情报来监视行为，以检测在 {{site.data.keyword.containerlong_notm}} 集群上运行的潜在被泄容器。
{: shortdesc}

从 2019 年 1 月 20 日开始，Network Insights (beta) 将替换 Network Analytics 功能。这将删除服务仪表板中的所有分析卡，但发现结果仍会保留在发现结果数据库中。
{: important}

## 开始之前
{: #network-prereq}

如果当前在使用 Network Analytics 功能，那么必须先[删除服务组件](/docs/services/security-advisor?topic=security-advisor-setup-network#uninstall-analytics)，然后才能安装 Network Insights。要开始使用 Network Insights，请确保您满足以下先决条件。

- 如果您是在 Windows 10 上工作，请激活“适用于 Linux 的 Windows 子系统”，然后安装 [Ubuntu shell](https://win10faq.com/install-run-ubuntu-bash-windows-10/){: external}。
- 安装 yq CLI：
  * 对于 [macOS 和 Windows 10](http://mikefarah.github.io/yq/){: external}。
  * 对于 CentOS、Red Hat 和 Ubuntu，请运行以下命令来安装 V1.15。
    ```
    wget https://github.com/mikefarah/yq/releases/download/1.15.0/yq_linux_amd64       
    mv yq_linux_amd64 yq   
    chmod +x yq    
    mv yq /usr/local/bin/     
    yq -V
    ```
    {: codeblock}     
- 更新的 cURL 二进制文件：对于 CentOS 和 Red Hat，可以通过运行 `yum update -y nss curl libcurl` 进行更新。
- [{{site.data.keyword.cloud_notm}} CLI 和必需的插件](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)
- [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/){: external} V1.10.11 或更高版本
- [Kubernetes Helm（包管理器）](/docs/containers?topic=containers-helm)V2.9.0 或更高版本。
- 标准 Kubernetes V1.10.11 或更高版本集群

## 创建 COS 存储区
{: #network-setup-cos}

通过使用 {{site.data.keyword.security-advisor_short}} GUI，可以创建新的 COS 实例和存储区。

1. 在服务仪表板的**集成**选项卡上，将 Network Insights 框中的**分析已禁用**切换为**分析已启用**。

2. 单击**转至设置**。

3. 在“先决条件”部分中，单击**创建 COS 实例和存储区**。这将自动创建遵循适当的命名约定并具有 IAM 许可权的 COS 实例和存储区。

如果您具有 COS 和存储区的现有实例，请确保它使用命名约定：`sa.<account_id>.telemetric.<cos_region>`。要允许服务读取 COS 实例中存储的数据，请使用 {{site.data.keyword.cloud_notm}} IAM 来设置[服务到服务授权](/docs/iam?topic=iam-serviceauth)。将 `source` 设置为 `{{site.data.keyword.security-advisor_short}}`，将 `target` 设置为您的 COS 实例。分配 IAM 角色`读取者`。


## 安装 {{site.data.keyword.security-advisor_short}} 组件
{: #network-install-components}

可以安装代理程序以从 Kubernetes 集群收集网络流日志。这些日志存储在 Cloud Object Storage 实例中。然后，可以启用 Network Insights 来分析日志，并在检测到可疑网络活动后向您发出警报。
{: shortdesc}

确保对要监视的每个集群重复此安装。
{: note}

1. 登录到 {{site.data.keyword.cloud_notm}} CLI。按照 CLI 中的提示完成登录。

  ```
  ibmcloud login -a cloud.ibm.com -r <region>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>区域</th>
      <th>端点</th>
    </tr>
    <tr>
      <td>英国</td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>美国南部</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

2. 设置集群的上下文。

  1. 获取命令以设置环境变量并下载 Kubernetes 配置文件。

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. 复制以 `export` 开头的输出，并将其粘贴到终端中，以设置 `KUBECONFIG` 环境变量。

3. 获取 Kubernetes 集群的版本。

  ```
  kube_version=$(kubectl version --output json) echo $(echo $kube_version | yq r - serverVersion.major).$(echo $kube_version | yq r - serverVersion.minor)
  ```
  {: codeblock}

3. 如果使用的是 Kubernetes Service V1.12.x，请使用以下命令安装 Helm。如果使用的是其他版本，请查看 Kubernetes 文档，了解应该执行哪些安装步骤[在具有公共访问权的集群中设置 Helm](/docs/containers?topic=containers-helm#public_helm_install)。

  1. 删除任何现有部署。

    ```
    kubectl delete deployment tiller-deploy -n kube-system
    ```
    {: codeblock}

  2. 将 Tiller RBAC 策略应用于部署。

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: codeblock}
  
  3. 在集群中初始化 Helm，然后在集群中安装 Tiller。

    ```
    helm init --service-account tiller
    ```
    {: codeblock}

  4. 通过验证 `tiller-deploy` pod 的状态是否为 `running`，检查安装是否成功。

    ```
    kubectl get pods -n kube-system -l app=helm
    ```
    {: codeblock}

4. 确保 Kubernetes Service 的版本为 1.11 或更高版本，然后将以下存储库克隆到本地系统。

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-network-insights.git
  ```
  {: codeblock}

  Kubernetes Service V1.10 已不推荐使用且不受支持。如果已安装 V1.10 或更高版本，请通过运行以下 Helm 命令重新启动分析器 pod 来修复现有映像中的漏洞：`helm upgrade --recreate-pods network-insights`。
  {: deprecated}

5. 切换至 `security-advisor-network-insights` 文件夹。

6. 切换至 `v1.10+` 目录。

7. 通过运行以下命令来解压缩 `.tar` 文件。

  ```
  tar -xvf security-advisor-network-insights.tar
  ```
  {: codeblock}

8. 切换至 `security-advisor-network-insights` 文件夹。

9. 使用 [Kubernetes Service 集成文档](/docs/containers?topic=containers-helm)来安装 Helm。

10. 可选：[启用 TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md){: external}。如果要使用工作站来处理在多个集群中安装分析组件的操作，并且启用了 TLS，请确保 TLS 配置是最新的，并与计划在其中安装组件的当前集群相匹配。

11. 运行以下命令来安装 Helm chart 及其依赖项。该命令将验证存储区是否使用了正确的命名约定，创建 Kubernetes 私钥，使用集群 GUID 更新值以及部署 Network Insights Helm chart。如果遇到错误，请尝试运行 `helm init --upgrade`。

  ```
  ./network-insight-install.sh <cos_region> <cos_api_key>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>变量</th>
      <th>描述</th>
    </tr>
    <tr>
      <td><code>cos_region</code></td>
      <td>部署 COS 实例的区域。选项包括：<code>us-south</code> 和 <code>eu-gb</code>。</td>
    </tr>
    <tr>
      <td><code>cos_api_key</code></td>
      <td>创建用于访问 COS 实例和存储区的 [API 密钥](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials)。该密钥必须具有`写入者`平台角色。</td>
    </tr>
  </table>

您已将集群成功配置为使用 {{site.data.keyword.security-advisor_short}} Network Insights！



## 删除组件
{: #network-delete}

如果您不再需要使用 Network Insights，那么可以从集群中删除服务组件。
{: shortdesc}

1. 登录到 {{site.data.keyword.cloud_notm}} CLI。按照 CLI 中的提示完成登录。

  ```
  ibmcloud login -a cloud.ibm.com -r <region>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>区域</th>
      <th>端点</th>
    </tr>
    <tr>
      <td>英国</td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>美国南部</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

2. 设置集群的上下文。

  1. 获取命令以设置环境变量并下载 Kubernetes 配置文件。

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. 复制以 `export` 开头的输出，并将其粘贴到终端中，以设置 `KUBECONFIG` 环境变量。

3. 使用 Helm 删除组件。

  ```
  helm del --purge network-insights [--tls]
  ```
  {: codeblock}

4. 删除 Kubernetes 私钥。

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}

确保对要从中除去代理程序的每个集群执行此删除过程。
{: tip}

## 卸载 Network Analytics
{: #uninstall-analytics}

如果使用的是 Beta 版本的 Network Analytics，那么必须先卸载旧的 {{site.data.keyword.security-advisor_short}} 组件，然后才能安装新组件。请确保对包含任何服务组件的每个集群重复此过程。
{: shortdesc}

1. 登录到 {{site.data.keyword.cloud_notm}}。

  ```
  ibmcloud login -a https://api.us-south.ibm.cloud.com --sso
  ```
  {: codeblock}

2. 列出帐户中的所有集群以获取集群的名称。

  ```
  ibmcloud ks clusters
  ```
  {: codeblock}

3. 设置集群的上下文。

  1. 获取命令以设置环境变量并下载 Kubernetes 配置文件。

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. 复制以 `export` 开头的输出，并将其粘贴到终端中，以设置 `KUBECONFIG` 环境变量。

4. 导航至解压缩的归档位置，然后运行卸载程序脚本。

  ```
  ./uninstall.sh
  ```
  {: codeblock}

5. 可选：从集群中卸载 Helm 服务器组件。

  ```
  helm reset
  ```
  {: codeblock}
