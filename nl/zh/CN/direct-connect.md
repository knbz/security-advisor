---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

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


# 直接连接
{: #setup-custom-gui}

通过 {{site.data.keyword.security-advisor_short}}，您可以集成现有的定制安全工具，无论是开放式源代码、定制开发还是第三方服务均可。您可以通过将相应 URL 添加到集成列表来创建从 {{site.data.keyword.security-advisor_short}} 到其他工具的直接连接。通过创建链接，可以轻松监视使用的所有工具。
{: shortdesc}


## 开始之前
{: #custom-before-gui}

要能够添加集成，您必须首先具有要集成的合作伙伴的帐户。

{{site.data.keyword.security-advisor_short}} 不会持久存储与合作伙伴服务相关的任何凭证。企业用户必须使用 SAML 向 {{site.data.keyword.cloud_notm}} 和业务合作伙伴进行认证。
{: note}

## 配置连接
{: #custom-configure-connection}

1. 登录到安全工具并获取唯一 URL。

2. 使用控制台登录到 {{site.data.keyword.cloud_notm}}。

3. 单击**定制集成 > 直接连接**。这将显示一个屏幕。

  1. 为解决方案提供名称。只能在名称中使用字母数字字符、空格和短划线 (-)。

  2. 输入解决方案的 URL，格式为：`www.<website>.<domain>`。

  3. 上传用于表示工具的图标或图像。

  4. 单击**连接**以完成配置。{{site.data.keyword.security-advisor_short}} 将创建集成所需的工件，例如服务标识、API 密钥、帐户标识和元数据。分配的角色是 `writer`。
