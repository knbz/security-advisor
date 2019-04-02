---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-13"

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

# 利用预集成的服务
{: #setup-services}

{{site.data.keyword.security-advisor_short}} 随附了多个预填充的卡，可帮助您监视安全风险和威胁。
{: shortdesc}

{{site.data.keyword.security-advisor_short}} 将为以下服务自动创建卡：

* {{site.data.keyword.registrylong_notm}}
* {{site.data.keyword.cloudcerts_long_notm}}

虽然您无需执行任何操作来创建 {{site.data.keyword.security-advisor_short}} 与服务之间的连接，但您必须具有已使用信息进行配置的服务实例。


## 监视容器映像中的漏洞
{: #setup-images}

通过 {{site.data.keyword.registryshort_notm}}，您有权访问漏洞顾问程序，该程序会持续扫描 {{site.data.keyword.registryshort_notm}} 实例中的映像，以确定潜在的安全问题。如果发现了问题，将向您发送警报，并且您可以在 {{site.data.keyword.security-advisor_short}} 仪表板中查看综合报告。
{:shortdesc}

了解有关 [{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-index#index) 的更多信息。


**开始之前**

在开始使用注册表之前，请确保您已安装以下 CLI 和插件：
* [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)
* Container Registry 插件

  ```
  ibmcloud plugin install container-registry -r Bluemix
  ```
  {: pre}


**创建名称空间**

1. 使用 CLI 登录到您的帐户。

  ```
  ibmcloud login --sso
  ```
  {: pre}

2. 登录到 {{site.data.keyword.registryshort_notm}}。

  ```
  ibmcloud cr login
  ```
  {: pre}

3. 可选：创建名称空间。您始终可以使用现有名称空间。

  ```
  ibmcloud cr namespace-add
  ```
  {: pre}

3. 对映像进行标记。

  ```
  docker tag <image>:<tag> registry.ng.bluemix.net/<namespace>/<image>:<tag>
  ```
  {: pre}

5. 推送映像。

  ```
  docker push registry.ng.bluemix.net/<namespace>/<image>:<tag>
  ```
  {: pre}


将映像推送到 {{site.data.keyword.registryshort_notm}} 名称空间后，有关发现的任何漏洞的信息会显示在服务仪表板的**具有漏洞的映像**卡中。您还可以向下钻取到特定映像以了解更多信息，例如所有识别到的漏洞和配置问题的描述。


## 监视证书
{: #setup-certificates}

您知道 {{site.data.keyword.cloudcerts_long_notm}} 可以帮助监视和管理 SSL/TLS 证书吗？通过集成 {{site.data.keyword.cloudcerts_short}} 和 {{site.data.keyword.security-advisor_short}}，您可以提前获得有关证书到期的警报，这可帮助防止服务或应用程序中断。
{:shortdesc}

根据上传到 {{site.data.keyword.cloudcerts_short}} 的证书的到期时间数据，发现结果会在证书到期前的 90 天、60 天、10 天和 1 天，显示在 {{site.data.keyword.security-advisor_short}} 仪表板中。此外，从证书到期的第一天开始，您还会每天收到有关到期证书的通知。

要触发手动更新，您可尝试将在一天后到期的证书上传到 {{site.data.keyword.cloudcerts_short}} 实例。成功导入后，您可以看到关键风险指标 (KRI) 和发现结果显示在 {{site.data.keyword.security-advisor_short}} 仪表板中。

您可以通过阅读文档来了解有关 [{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager?topic=certificate-manager-gettingstarted#gettingstarted) 的更多信息。
{: tip}

**创建证书**

要创建在一天后到期的自签名证书，可以在终端中运行以下 OpenSSL 命令。

```
openssl req -x509 -newkey rsa:2048 -keyout key.pem -subj "/CN=myservice.com" -out server.pem -days 1 -nodes
```
{: pre}


**上传证书**

1. 在 {{site.data.keyword.Bluemix_notm}}“目录”中，搜索“{{site.data.keyword.cloudcerts_short}}”。
2. 为服务实例提供名称或使用预设名称。
3. 单击**创建**。
4. 要将组织证书导入到 {{site.data.keyword.cloudcerts_short}}，请单击**导入证书**。

既然证书已导入，现在到期时间和到期证书等信息会显示在 {{site.data.keyword.security-advisor_short}} 仪表板中的**证书**卡上。通过单击该卡，可以获取有关证书的更具体信息，例如证书所属的服务实例。您还可以查看为解决安全漏洞而可以执行的任何步骤。

要停止通知，您必须更新证书，将证书上传到 {{site.data.keyword.cloudcerts_short}}，然后删除到期的证书。
{: tip}
