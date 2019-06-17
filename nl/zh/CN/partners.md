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


# 合作伙伴
{: #setup-partner}

通过 IBM 业务合作伙伴集成，可将来自第三方提供者的关键警报和发现结果引入 {{site.data.keyword.security-advisor_long}} 仪表板。这些合作伙伴与 IBM 合作，帮助创建和简化您的集成体验，并提供相关指导。
{: shortdesc}

## 开始之前
{: #partner-before}

在开始集成合作伙伴之前，请确保您满足以下先决条件。

* 确保您具有要集成的合作伙伴的帐户。
* 确保您具有必需的管理许可权，可生成合作伙伴服务的集成 URL。
* 确保您具有对 {{site.data.keyword.cloud_notm}} 的 IAM 管理访问权以及对 {{site.data.keyword.security-advisor_short}} 的管理者访问权。

## 集成向导
{: #wizard}

作为同时具有 {{site.data.keyword.cloud_notm}} 和伙伴帐户中管理许可权的管理员，您可以使用集成向导来快速启动并开始运行。该向导包含四个基本步骤：

* 建立信任，并将 {{site.data.keyword.cloud_notm}} 与伙伴帐户相关联
* 在帐户之间复制必需的信息，例如凭证和 URL
* 将合作伙伴的发现结果元数据安装到 {{site.data.keyword.security-advisor_short}} 中
* 通过将来自合作伙伴的发现结果发布到 {{site.data.keyword.security-advisor_short}} 中来验证配对情况


## 集成 NeuVector
{: #setup-neuvector}

通过 [NeuVector](https://neuvector.com){: external}，可以检测网络威胁和违例，以阻止对基于容器的应用程序的攻击。借助监视，可以通过检测容器和主机中的 root 用户特权升级、端口扫描、逆向 shell 和可疑文件系统活动，防止漏洞利用和爆发。
{: shortdesc}

要将 NeuVector 与 {{site.data.keyword.security-advisor_short}} 集成，可以使用以下步骤：

1. 浏览至**集成 > 合作伙伴集成**，然后从提供的选项中选择 **NeuVector**。
2. 连接帐户。
  1. 提供连接的名称。
  2. 提供 NeuVector 仪表板 URL。
  3. 提供 NeuVector 配置 URL。
    1. 登录到 NeuVector 并导航至设置。
    2. 单击**生成 URL**。
    3. 复制该 URL 并将其粘贴到 {{site.data.keyword.security-advisor_short}} 集成向导中。
  4. 单击**下一步**。
3. 验证您是否授予有对服务的许可权，可生成服务标识和 API 密钥，然后通过单击**下一步**来创建与服务的连接。
4. 单击**完成**。
5. 转至服务仪表板，以复查 NeuVector 已发送到 {{site.data.keyword.containershort_notm}} 的测试发现结果。



## 集成 Twistlock
{: #setup-twistlock}

可以通过阻止在环境中部署易受攻击的映像来防范风险。通过自动化和定制策略强制实施，[Twistlock](https://www.twistlock.com){: external} 提供了对应用程序生命周期中每个阶段的完全控制。
{: shortdesc}

配置合作伙伴连接时，将在仪表板中创建两个卡，用于概述来自的 Twistlock 的发现结果。

**Twistlock 运行时**引入了两个关键风险指标 (KRI)：

* 突发事件和审计总数：与云本机工作负载上的突发事件或审计相关的发现结果数。
* 防火墙审计总数：与防火墙问题相关的发现结果数。

**Twistlock 漏洞**引入了一个 KRI：

* 具有新漏洞的映像总数：与容器映像中找到的漏洞相关的发现结果数。

您可以在 Twistlock 文档中了解有关该公司的更多信息。

### 配置 Twistlock
{: #configure-twistlock}

1. 在 {{site.data.keyword.security-advisor_short}} 仪表板中，导航至**集成 > 合作伙伴集成**，然后从提供的选项中选择 **Twistlock**。

2. 单击**是，将我的帐户连接到 {{site.data.keyword.security-advisor_short}}**。

  如果您还没有帐户，请单击**否，帮助我创建帐户 > 创建帐户**。您可以填写自己的信息，随后 Twistlock 销售团队将联系您以开始创建帐户。
  {: note}

3. 为连接提供名称。请确保该名称对于您的帐户唯一，并且只使用字母数字字符、空格或连字符。

4. 可选：如果您还没有配置 URL，请单击**生成注册 URL** 以转至 Twistlock 文档，并了解如何创建 URL。确保您具有许可证随附的 Twistlock 令牌，以获取对文档的访问权。

5. 在 Twistlock 仪表板中，导航至**管理 > 警报**选项卡，然后单击**添加概要文件**。

6. 从**提供者**下拉列表中选择 **{{site.data.keyword.security-advisor_short}}**。

7. 单击**复制**以使用提供的配置 URL。

8. 返回到 {{site.data.keyword.security-advisor_short}} 仪表板，将该 URL 粘贴到**输入 Twistlock 配置 URL** 框中。

9. 单击**连接合作伙伴**。

### 验证连接
{: #twistlock-verify}

1. 在 {{site.data.keyword.security-advisor_short}} 仪表板中，检查 Twistlock 卡是否按预期显示。

2. 在 Twistlock 仪表板中，刷新**警报**选项卡。这将显示 {{site.data.keyword.security-advisor_short}} 连接。打开该连接。

3. 验证要通知的警报类型是否已选中，然后单击**验证**以确保连接完成。
