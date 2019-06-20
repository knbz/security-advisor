---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-06"

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


# Network Insights (beta)
{: #network}

通过 {{site.data.keyword.security-advisor_long}}，您可以基于针对在 {{site.data.keyword.containerlong_notm}} 集群上运行的潜在被泄容器的威胁情报、行为模式和机器学习来获取相关洞察。
{: shortdesc}


## 工作方式
{: #network-how-it-works}

Network Insights 功能是 {{site.data.keyword.security-advisor_short}} 服务的一个附加组件。启用并配置此功能后，将收集并持续监视和分析集群与外部实体之间的集群网络通信（入局和出局）。

请查看下图以了解信息流。

![Network Insights 流程图](images/network-insights-flow.png)

1. 作为帐户管理员，您可以将 Network Insights 安装到要监视的每个集群中。
2. 网络流日志会转发到 Cloud Object Storage 存储区并在其中进行存储，直到您决定将这些日志删除为止。使用 {{site.data.keyword.security-advisor_short}} GUI 来创建存储区时，系统会分配服务到服务 IAM 角色，以便该服务可以查看日志。
3. 在启用了 Network Insights 的情况下，系统会分析 COS 存储区中的原始数据，以确定是否有可疑行为。
4. 在标记了一个可能的安全问题后，会将发现结果转发到“发现结果”数据库。
5. 发现结果会显示在服务仪表板的三个卡上：
  * 可疑入站流量
  * 可疑出站流量
  * 流量中的异常


## 收集数据
{: #network-data}

{{site.data.keyword.security-advisor_short}} 提供了一个收集器，用于收集集群和外部实体之间发生的网络流信息。
{: shortdesc}

收集的原始数据会存储在 Cloud Object Storage 存储区中，您可以确定数据在其中存储的时间长度。您对收集的数据具有所有权和控制权，这意味着存储、保护和删除这些数据将由您负责。{{site.data.keyword.security-advisor_short}} 会将发现结果保留 90 天。在此期间，结果会显示在服务仪表板的 Network Insights 卡上。因此，尽管 90 天后在仪表板中无法再看到发现结果，但原始数据仍可能位于存储器中。

将收集以下信息：

* 正在通信的同级的 IP 地址
* 同级使用的端口
* 正在使用的协议集
* 传输的数据量
* 一组特定于协议的参数
* 各种时间戳记。

不会收集已交换的实际数据。
{: tip}

从安全角度来说，如果法律或公司需求允许删除收集的数据，那么最好是清除这些数据。有关更多信息，请查看[删除对象](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion)。



## 网络：可疑入站流量
{: #network-suspicious-inbound}

{{site.data.keyword.security-advisor_short}} 会在服务仪表板的**可疑入站流量**卡上通知您有外部客户机尝试调查和危害集群。
{: shortdesc}


对于 IBM X-Force 分类为用作扫描程序、用作僵尸网络一部分、用于挖掘加密货币或用于匿名化服务的分发恶意软件的客户机，系统将持续监视所有这些客户机的行为模式。如果该类型的客户机访问受监视集群，并表现出令人警惕的行为，Network Insights 会发出发现结果。


该卡引入了两个关键风险指标 (KRI)：

* 可疑客户机执行的侦查：包含与访问集群的客户机相关的发现结果数。
* 可疑客户机发送的异常大的有效内容：包含与客户机和集群之间发送的数据量相关的发现结果数。异常有效内容是与集群不吻合的任何内容。


发现结果可能包含具有以下行为的可疑客户机：

* 将异常大量的数据发送到集群。
* 对集群服务执行异常大量的请求。
* 根据客户机执行的集群调查尝试次数所示，似乎客户机将集群设定为目标。



## 网络：可疑出站流量
{: #network-suspicious-outbound}

{{site.data.keyword.security-advisor_short}} 会在服务仪表板的**可疑出站流量**卡中通知您有任何潜在被泄容器在 {{site.data.keyword.containershort_notm}} 集群上运行。
{: shortdesc}

对于 IBM X-Force 分类为用作扫描程序、用作僵尸网络一部分、用于挖掘加密货币或用于匿名化服务的分发恶意软件的客户机，该服务将持续监视访问这些客户机的容器的行为模式。在受监视集群上的容器访问可疑同级并表现出令人警惕的行为后，Network Insights 会发出发现结果。

该卡引入了两个关键风险指标 (KRI)：

* 对可疑服务器的出站访问：与访问这些服务器的集群相关的发现结果数。
* 与可疑服务器交换的异常大的有效内容：包括与客户机和集群之间发送的数据量相关的发现结果数。


发现结果可能包含具有以下行为的容器：

* 将异常大量的数据发送到可疑服务器。
* 从可疑服务器抽取大量数据。
* 对可疑服务器执行大量请求。


## 网络：流量中的异常（实验）
{: #network-anomalies}

在此实验功能中，{{site.data.keyword.security-advisor_short}} 会监视网络以学习行为模式。在构成模式后，会在服务仪表板的**流量中的异常**卡上对显示为异常的任何行为进行标记和概述。
{: shortdesc}

通过监视容器与其同级之间的行为模式，可创建正常容器行为的模型。捕获模型时，会监视其中是否有特定更改。如果表现出令人警惕的更改，那么 Network Insights 将发出发现结果。

该卡引入了两个关键风险指标 (KRI)：

* 高于正常的侦察或数据交换活动：与在集群和任何外部同级之间检测到的异常交互相关的发现结果数。
* 对新服务器的出站访问：与集群访问的新检测到的服务器相关的发现结果数。

发现结果可能包含：  

* 访问先前从未访问过的服务器的容器。
* 发往特定同级或从中使用的数据量明显超过正常水平的容器。
* 对特定容器的调查水平显著增加。

在开发模型后，会检测与已学习模型的偏差，并且在表现出令人警惕的更改时，Network Insights 会将发现结果发布到 Security Advisor 仪表板。发现结果在“网络：流量中的异常”卡中进行概述。该卡引入了两个关键风险指标 (KRI)。“高于正常的侦察或数据交换活动”KRI 对与在集群和外部同级之间检测到的异常交互相关的发现结果计数。“对新服务器的出站访问”KRI 对与集群访问的新检测到的服务器相关的发现结果计数。  

## 后续步骤
{: #network-next}

准备好开始了吗？请查看[启用 Network Insights](/docs/services/security-advisor?topic=security-advisor-setup-network)！
