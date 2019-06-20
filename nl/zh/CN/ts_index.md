---

copyright:
  years: 2019
lastupdated: "2019-06-06"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# 故障诊断
{: #troubleshooting}

在使用 {{site.data.keyword.security-advisor_long}} 时，如果遇到问题，可以考虑使用下列方法进行故障诊断和获取帮助。
{: shortdesc}


## 获取帮助与支持
{: #getting-help-and-support}



您可以通过搜索信息或通过在论坛中提问来获取帮助。您还可以开具支持凭单。使用论坛提问时，请标记您的问题，以便 {{site.data.keyword.cloud_notm}} 开发团队可以看到您的问题。
{: shortdesc}

  * 如果有关于 {{site.data.keyword.security-advisor_short}} 的技术问题，请在 [Stack Overflow](https://stackoverflow.com/){: external} 上发帖提问。确保包含 `security-advisor` 和 `ibm-cloud` 标记。

  * 有关服务和入门指示信息的问题，请使用 [dW Answers](https://developer.ibm.com/){: external} 论坛。确保包含 `security-advisor` 和 `ibm-cloud` 标记。


有关获取支持的更多信息，请参阅[如何获得所需的支持](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)。


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
在安装其中一个内置的 Insights 产品之前，必须先安装 Helm。可以使用 [Kubernetes Service 集成文档](/docs/containers?topic=containers-helm)来安装 Helm。


## 已知问题：无法创建定制解决方案事件实例
{: #ts-custom-occurrence}

{: tsSymptoms}
您尝试创建定制解决方案事件实例，但信息未显示在浏览器中，并且您不会收到任何错误消息。

{: tsCauses}
创建事件实例失败，因为您选择的名称已在使用。

{: tsResolve}
请为事件实例选择其他名称。

## 已知问题：直接连接映像未更新
{: #ts-known-cant-edit}

{: tsSymptoms}
上传或编辑直接连接的映像后，此映像未立即显示。

{: tsCauses}
映像不正确显示是已知问题。

{: tsResolve}
要查看更新的映像，请刷新浏览器。这样新映像会显示在直接连接表中。

