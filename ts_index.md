---

copyright:
  years: 2019
lastupdated: "2019-02-07"

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

# Troubleshooting
{: #troubleshooting}

If you have problems while you're working with {{site.data.keyword.security-advisor_long}}, consider these techniques for troubleshooting and getting help.
{: shortdesc}


## Getting help and support
{: #getting-help-and-support}



You can find help by searching for information or by asking questions through a forum. You can also open a support ticket. When you are using the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.Bluemix_notm}} development teams.
{: shortdesc}

* If you have technical questions about {{site.data.keyword.security-advisor_short}}, post your question on <a href="http://stackoverflow.com/search?q=ibm+" target="_blank">Stack Overflow <img src="../../icons/launch-glyph.svg" alt="External link icon"></a>. Be sure to include the `security-advisor` and `ibm-cloud` tags.
* For questions about the service and getting started instructions, use the <a href="https://developer.ibm.com/answers/search.html?f=&type=question&redirect=search%2Fsearch&sort=relevance&q=appid%20[bluemix]" target="_blank">dW Answers <img src="../../icons/launch-glyph.svg" alt="External link icon"></a> forum. Be sure to include the `security-advisor` and `ibm-cloud` tags.

For more information about getting support, see [how do I get the support that I need](/docs/get-support/howtogetsupport.html#getting-customer-support).

</br>

## You can't create a custom solution occurrence
{: #ts-custom-occurrence}

{: tsSymptoms}
You try to create a custom solution occurrence but the information doesn't show in a browser and you receive no error message.

{: tsCauses}
You have encountered a known issue. Creating the occurrence failed because the name that you chose already exists.

{: tsResolve}
Choose another name for your occurrence.

</br>

## KPI is missing
{: #ts-kpi-missing}

{: tsSymptoms}
You do not see any KRI on your **Suspicious server IPs card**.

{: tsCauses}
You have encountered a known issue.

{: tsResolve}
This issue cannot be resolved.

</br>

## The latest details do not show
{: #ts-latest-details}

{: tsSymptoms}
You do not see the latest details on your **Images with vulnerabilities** card.

{: tsCauses}
You have encountered a known issue. Occasionally, the information does not update the first time a page loads.

{: tsResolve}
To resolve the issue, click the refresh icon.

## Error: namespaces "security-advisor-insights" is forbidden
{: #ts-error-helm-install}

{: tsSymptoms}
When you try to install Network or Activity Insights, you encounter the error:

```
namespaces “security-advisor-insights” is forbidden: User “system:serviceaccount:kube-system:default” cannot get resource “namespaces” in API group “” in the namespace “security-advisor-insights”
```
{: screen}

{: tsCauses}
The `kube-system` default service account does not have admin access in your cluster. Prior to installing one of the Built-in Insights offerings, you must install Helm.

{: tsResolve}
To resolve the issue, install Helm with TLS enabled. If you're using your workstation to handle the installation of analytics components in multiple clusters and TLS is enabled, be sure that the TLS configurations are current and match the current cluster where you plan to install the components.

  1. Install Helm by using the [Kubernetes Service integration docs](/docs/containers/cs_integrations.html#helm).

  2. [Enable TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md).
