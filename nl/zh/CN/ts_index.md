---

copyright:
  years: 2019
lastupdated: "2019-02-18"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# 故障诊断
{: #troubleshooting}

在使用 {{site.data.keyword.security-advisor_long}} 时，如果遇到问题，可以考虑使用下列方法进行故障诊断和获取帮助。
{: shortdesc}


## 获取帮助与支持
{: #getting-help-and-support}



您可以通过搜索信息或通过在论坛中提问来获取帮助。您还可以开具支持凭单。使用论坛提问时，请标记您的问题，以便 {{site.data.keyword.Bluemix_notm}} 开发团队可以看到您的问题。
{: shortdesc}

* 如果有关于 {{site.data.keyword.security-advisor_short}} 的技术问题，请在 <a href="http://stackoverflow.com/" target="_blank">Stack Overflow <img src="../../icons/launch-glyph.svg" alt="外部链接图标"></a> 上发帖提问。确保包含 `security-advisor` 和 `ibm-cloud` 标记。
* 有关服务和入门指示信息的问题，请使用 <a href="https://developer.ibm.com/" target="_blank">dW Answers <img src="../../icons/launch-glyph.svg" alt="外部链接图标"></a> 论坛。确保包含 `security-advisor` 和 `ibm-cloud` 标记。

有关获取支持的更多信息，请参阅[如何获得所需的支持](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)。


## 无法创建定制解决方案事件实例
{: #ts-custom-occurrence}

{: tsSymptoms}
您尝试创建定制解决方案事件实例，但信息未显示在浏览器中，并且您不会收到任何错误消息。

{: tsCauses}
您遇到的是已知问题。创建事件实例失败，因为您选择的名称已存在。

{: tsResolve}
请为事件实例选择其他名称。


## 错误：名称空间“security-advisor-insights”被禁止
{: #ts-error-helm-install}

{: tsSymptoms}
尝试安装 Network Insights 或 Activity Insights 时，遇到以下错误：

```
namespaces “security-advisor-insights” is forbidden: User “system:serviceaccount:kube-system:default” cannot get resource “namespaces” in API group “” in the namespace “security-advisor-insights”
```
{: screen}

{: tsCauses}
`kube-system` 缺省服务帐户在集群中没有管理员访问权。

{: tsResolve}
在安装其中一个内置的 Insights 产品之前，必须先安装 Helm。可以使用 [Kubernetes Service 集成文档](/docs/containers?topic=containers-integrations#helm)来安装 Helm。


## 已知缺陷：某些 Network Insights 发现结果不显示
{: #ts-network-analytics}

{: tsSymptoms}
通过仪表板或 API 查找 Network Insights 时，不会看到所有发现结果类型。

{: tsCauses}
某些 Network Insights 发现结果类型仅在 IBM Cloud Kubernetes Service V1.10 或更低版本上有效。

{: tsResolve}
请使用 Kubernetes Service V1.10 或更低版本。
