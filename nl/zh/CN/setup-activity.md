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

# Activity Insights
{: #setup-activity}

通过 {{site.data.keyword.security-advisor_long}}，您可以持续监视 {{site.data.keyword.cloud_notm}} Activity Tracker 日志，以识别资源中的未经授权或可疑的行为和更改。可以使用基于服务提供的最佳实践的规则包，也可以创建您自己的定制规则。
{: shortdesc}


## 开始之前
{: #activity-prereq}

要开始使用 Activity Insights，请确保您满足以下先决条件。

- 如果您是在 Windows 10 上工作，请激活“适用于 Linux 的 Windows 子系统”，然后安装 [Ubuntu shell](https://win10faq.com/install-run-ubuntu-bash-windows-10/)
- 安装 yq CLI：
  * 对于 [macOS 和 Windows 10](http://mikefarah.github.io/yq/)。
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
- [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/) V1.10.11 或更高版本
- [Kubernetes Helm（包管理器）](/docs/containers?topic=containers-integrations#helm)V2.9.0 或更高版本。
- 标准 Kubernetes V1.10.11 或更高版本集群


## 创建 COS 存储区
{: #activity-setup-cos}

通过使用 {{site.data.keyword.security-advisor_short}} GUI，可以创建新的 COS 实例和存储区。

1. 导航至服务仪表板的**集成**选项卡。

2. 将 Activity Insights 框中的**分析已禁用**切换为**分析已启用**。

3. 单击**转至设置**。

4. 在“先决条件”部分中，单击**创建 COS 实例和存储区**。这将自动创建遵循适当的命名约定并具有 IAM 许可权的 COS 实例和存储区。这将显示存储区信息。

如果您具有 COS 和存储区的现有实例，请确保它使用命名约定 `sa.<account_id>.telemetric.<cos_region>`。要允许服务读取 COS 实例中存储的数据，请使用 {{site.data.keyword.cloud_notm}} IAM 来设置[服务到服务授权](/docs/iam?topic=iam-serviceauth#serviceauth)。将 `source` 设置为 `{{site.data.keyword.security-advisor_short}}`，将 `target` 设置为您的 COS 实例。分配 IAM 角色`读取者`。


## 安装 {{site.data.keyword.security-advisor_short}} 组件
{: #activity-install-components}

可以安装代理程序以从 {{site.data.keyword.cloud_notm}} 帐户收集审计流日志。这些日志存储在 Cloud Object Storage 实例中，在其中可以启用 Activity Insights 来分析日志，以确定是否有可疑行为。
{: shortdesc}

1. 将以下存储库克隆到本地系统。

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. 切换至 `security-advisor-activity-insights` 文件夹。

3. 通过运行以下命令来解压缩 `.tar` 文件。

  ```
  tar -xvf security-advisor-activity-insights.tar
  ```
  {: codeblock}

4. 切换至 `security-advisor-activity-insights` 文件夹。
5. 登录到 {{site.data.keyword.cloud_notm}} CLI。按照 CLI 中的提示完成登录。

  ```
  ibmcloud login -a https://api.<region>.bluemix.net
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

6. 设置集群的上下文。

  1. 获取命令以设置环境变量并下载 Kubernetes 配置文件。

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. 复制以 `export` 开头的输出，并将其粘贴到终端中，以设置 `KUBECONFIG` 环境变量。

7. 使用 [Kubernetes Service 集成文档](/docs/containers?topic=containers-integrations#helm)来安装 Helm。

8. 可选：[启用 TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md)。如果要使用工作站来处理在多个集群中安装分析组件的操作，并且启用了 TLS，请确保 TLS 配置是最新的，并与计划在其中安装组件的当前集群相匹配。

9. 运行以下命令来安装 Insights。该命令将验证存储区的命名约定，创建 Kubernetes 私钥，使用集群 GUID 更新值以及部署 Activity Insights。

  ```
  ./activity-insight-install.sh <cos_region> <cos_api_key> <at_region> <account_api_key> <account_spaces>
  ```
  {: codeblock}

  如果遇到错误，请尝试运行 `helm init --upgrade`。
  {: tip}

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
      <td>创建用于访问 COS 实例和存储区的 [API 密钥](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials#service-credentials)。该密钥必须具有`写入者`平台角色。</td>
    </tr>
    <tr>
      <td><code>at_region</code></td>
      <td>在其中创建 COS 实例和存储区的区域。选项包括：<code>us-south</code> 和 <code>eu-gb</code>。</td>
    </tr>
    <tr>
      <td><code>account_api_key</code></td>
      <td>{{site.data.keyword.cloud_notm}} 帐户的平台 API 密钥。</td>
    </tr>
    <tr>
      <td><code>account_spaces</code></td>
      <td>{{site.data.keyword.cloud_notm}} 帐户的空间 GUID 的逗号分隔列表。</td>
    </tr>
  </table>


## 向 COS 添加规则包
{: #activity-adding-rules}

规则包是包含要监视的规则列表的 JSON 文件。可以下载规则包，也可以[创建您自己的规则包](/docs/services/security-advisor?topic=security-advisor-activity#activity-packages)。{{site.data.keyword.security-advisor_short}} 引擎会验证每个规则是否遵循正确的语法。
{: shortdesc}

1. 克隆以下存储库以获取多个预设规则包。将在本地系统上创建名称为 *security-advisor-ata-rule-packages* 的文件。

  ```
  https://github.ibm.com/security-services/security-advisor-ata-rule-packages.git
  ```
  {: codeblock}

2. 在本地创建名称为 *IBM.rules/activities* 的文件。

3. 将 JSON 文件从 *security-advisor-ata-rule-packages* 移至 *IBM.rules/activities*。

4. 导航至 {{site.data.keyword.cloud_notm}}“仪表板”，并选择与 Activity Insights 关联的 COS 服务实例。

5. 在服务仪表板的**存储区**选项卡上，选择与 Activity Insights 关联的存储区。

5. 在 COS 实例主页上，单击**上传**，然后选择**文件夹**。

6. 如果出现提示，请单击**安装 Aspera Connect 客户机**。如果看不到提示，说明您已安装该客户机。如果需要安装该客户机，请在安装完成时重复步骤 5。

7. 选择 *IBM.rules/activities* 文件夹。

8. 单击**上传**。

要使用您自己的包吗？请使用其中一个 JSON 文件作为指南，并创建符合您组织需要的规则。创建文件后，将其添加到 COS 实例中的 *IBM.rules/activities* 文件夹。有关规则类型和格式设置的更多信息，请查看[了解规则包](/docs/services/security-advisor?topic=security-advisor-activity#activity)。
{: tip}


## 删除组件
{: #activity-delete}

如果您不再需要使用 Activity Insights，那么可以从集群中删除服务组件。
{: shortdesc}

1. 使用 Helm 删除服务组件。如果启用了 TLS，请确保使用 `-tls` 标志。

  ```
  helm del --purge activity-insights [--tls]
  ```
  {: codeblock}

2. 删除 Kubernetes 私钥。

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}
