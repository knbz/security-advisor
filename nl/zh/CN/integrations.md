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


# 集成
{: #integrations}

通过 {{site.data.keyword.security-advisor_long}}，您可以集成其他内置洞察和合作伙伴解决方案，或者创建自己的定制集成。
{: shortdesc}


## 预集成的发现结果
{: #integrate-built-in}

通过 {{site.data.keyword.security-advisor_long}}，您可以利用内置服务功能来深入了解潜在问题。
{: shortdesc}


{{site.data.keyword.security-advisor_short}} 集成了以下开箱即用的功能：

* Certificate Manager，用于与证书到期和生命周期相关的通知。
* 漏洞顾问程序，用于获取有关映像漏洞和配置问题的详细信息。

要了解更多信息，请查看[利用预集成服务](/docs/services/security-advisor?topic=security-advisor-setup-services)！

</br>

## 合作伙伴集成
{: #integrate-partner}

合作伙伴集成通过在 {{site.data.keyword.security-advisor_short}} 与其他安全工具（与 IBM 合作）之间创建连接来确保无缝用户体验，从而增强了 {{site.data.keyword.cloud_notm}} 部署的安全性。
{: shortdesc}

目前，{{site.data.keyword.security-advisor_short}} 合作伙伴包括 NeuVector 和 Twistlock。

您是合作伙伴，并且有兴趣将自己的解决方案与 {{site.data.keyword.security-advisor_short}} 集成？请通过联系 Matt Ward (wardm@us.ibm.com) 与我们的团队取得联系！
{: tip}

### NeuVector
{: #integrate-neuvector}

[NeuVector](https://neuvector.com){: external} 为 Kubernetes 和 Red Hat OpenShift 提供了一个高度集成的自动化安全平台，允许您监视和保护容器网络流量。具体来说，是保护东-西网络流量。

通过 NeuVector，可以检测网络威胁和违例，以阻止对基于容器的应用程序的攻击。借助监视，可以通过检测容器和主机中的 root 用户特权升级、端口扫描、逆向 shell 和可疑文件系统活动，防止漏洞利用和爆发。

有关设置 NeuVector 实例的帮助，请参阅[合作伙伴解决方案](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-neuvector)。


### Twistlock
{: #integrate-twistlock}

[Twistlock](https://www.twistlock.com){: external} 通过阻止在环境中部署易受攻击的映像，能够以独特的方式防范整个 SDLC 中的风险。仪表板中访问所有内容。通过自动化和定制策略强制实施，提供了对应用程序生命周期中每个阶段的完全控制。Twistlock Intelligence Stream 直接从 30 多个上游项目、商业来源以及 Twistlock Labs 的专项研究中获取并聚集漏洞信息。

通过专注于提供覆盖您堆栈中所有层的最精确数据，您可以获得准确的可视性和最低的误报率。Twistlock 将这些数据与您实际部署中的情况（例如，哪些容器公开到因特网，哪些容器以高特权运行，哪些实施了其他安全缓解措施）相结合，因此您始终可以了解哪些漏洞在特定环境中最为严重。

有关设置 Twistlock 实例的帮助，请参阅[合作伙伴解决方案](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-twistlock)。
</br>


## 定制集成
{: #integrate-custom}

您可能已有可信赖的安全工具。您可以将该工具与 {{site.data.keyword.security-advisor_short}} 仪表板集成，以便您的所有安全信息全部集中在一个地方！
{: shortdesc}

### 创建直接连接
{: #create-direct-connect}

通过使用 {{site.data.keyword.security-advisor_short}} GUI，可以为内部和外部安全工具设置书签，以便可以通过一个位置访问 {{site.data.keyword.security-advisor_short}} 仪表板中的所有内容。例如，可以为 PagerDuty 设置书签。

### 将您自己的工具与 API 集成
{: #integrate-tools-api}

通过发现结果 API，可以将定制安全工具中的发现结果集成到 {{site.data.keyword.security-advisor_short}} 仪表板中。这可以是生成安全警报或发现结果并且也支持基于 API 的交互的任何定制或合作伙伴工具。

## 内置洞察
{: #integrate-insights}

利用内置洞察，可以通过持续监视集群和帐户日志来检测潜在问题。通过监视网络流量和用户活动，可以帮助确保 {{site.data.keyword.cloud_notm}} 资源持续受到保护。
{: shortdesc}

### Network Insights (beta)
{: #integrate-network-insights}

通过 Network Insights (beta)，可以监视和分析 Kubernetes 集群与外部实体之间的集群网络通信（入局和出局）。通过使用集成的威胁情报和异常检测，该服务可以识别侦察攻击和可能泄露的资产。要了解更多信息，请查看 [Network Insights](/docs/services/security-advisor?topic=security-advisor-network)。

### Activity Insights（预览）
{: #integrate-activity-insights}

通过 Activity Insights（预览），可以持续监视 {{site.data.keyword.cloud_notm}} Activity Tracker 日志，以识别用户或应用程序使用规则包进行的未经授权或可疑活动。可以使用服务提供的基于安全性最佳实践的规则包，也可以定制适用于您需求的规则。要了解更多信息，请查看 [Activity Insights](/docs/services/security-advisor?topic=security-advisor-activity)。
