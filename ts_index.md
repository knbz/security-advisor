---

copyright:
  years: 2019
lastupdated: "2019-02-12"

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

* If you have technical questions about {{site.data.keyword.security-advisor_short}}, post your question on <a href="http://stackoverflow.com/" target="_blank">Stack Overflow <img src="../../icons/launch-glyph.svg" alt="External link icon"></a>. Be sure to include the `security-advisor` and `ibm-cloud` tags.
* For questions about the service and getting started instructions, use the <a href="https://developer.ibm.com/" target="_blank">dW Answers <img src="../../icons/launch-glyph.svg" alt="External link icon"></a> forum. Be sure to include the `security-advisor` and `ibm-cloud` tags.

For more information about getting support, see [how do I get the support that I need](/docs/get-support/howtogetsupport.html#getting-customer-support).


## You can't create a custom solution occurrence
{: #ts-custom-occurrence}

{: tsSymptoms}
You try to create a custom solution occurrence but the information doesn't show in a browser and you receive no error message.

{: tsCauses}
You have encountered a known issue. Creating the occurrence failed because the name that you chose already exists.

{: tsResolve}
Choose another name for your occurrence.


## Error: namespaces "security-advisor-insights" is forbidden
{: #ts-error-helm-install}

{: tsSymptoms}
When you try to install Network or Activity Insights, you encounter the error:

```
namespaces “security-advisor-insights” is forbidden: User “system:serviceaccount:kube-system:default” cannot get resource “namespaces” in API group “” in the namespace “security-advisor-insights”
```
{: screen}

{: tsCauses}
The `kube-system` default service account does not have admin access in your cluster.

{: tsResolve}
Prior to installing one of the Built-in Insights offerings, you must install Helm. You can install Helm by using the [Kubernetes Service integration docs](/docs/containers/cs_integrations.html#helm).


## Known defect: Some Network Insights findings do not show
{: #ts-network-analytics}

{: tsSymptoms}
You do not see all of the finding types when you look for Network Insights through the dashboard or the API.

{: tsCauses}
Some Network Insights findings types work only on IBM Cloud Kubernetes Service version 1.10 or less.

{: tsResolve}
Use Kubernetes Service version 1.10 or less.
