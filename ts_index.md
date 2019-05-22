---

copyright:
  years: 2019
lastupdated: "2019-05-22"

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



You can find help by searching for information or by asking questions through a forum. You can also open a support ticket. When you are using the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.cloud_notm}} development teams.
{: shortdesc}

  * If you have technical questions about {{site.data.keyword.security-advisor_short}}, post your question on <a href="https://stackoverflow.com/" target="_blank">Stack Overflow <img src="../../icons/launch-glyph.svg" alt="External link icon"></a>. Be sure to include the `security-advisor` and `ibm-cloud` tags.

  * For questions about the service and getting started instructions, use the <a href="https://developer.ibm.com/" target="_blank">dW Answers <img src="../../icons/launch-glyph.svg" alt="External link icon"></a> forum. Be sure to include the `security-advisor` and `ibm-cloud` tags.


For more information about getting support, see [how do I get the support that I need](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).


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
Prior to installing one of the Built-in Insights offerings, you must install Helm. You can install Helm by using the [Kubernetes Service integration docs](/docs/containers?topic=containers-helm).

## Known issue: Direct connection image doesn't update
{: #ts-known-cant-edit}

{: tsSymptoms}
When you upload or edit an image for a direct connection it doesn't immediately display.

{: tsCauses}
There is a known issue with images displaying correctly.

{: tsResolve}
To see the updated image, refresh your browser. The new image will be displayed in the direct connection table.

