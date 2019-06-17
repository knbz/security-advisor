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

# Fehlerbehebung
{: #troubleshooting}

Treten bei der Arbeit mit {{site.data.keyword.security-advisor_long}} Probleme auf, lesen Sie die folgenden Informationen zur Fehlerbehebung und zum Anfordern von Hilfe.
{: shortdesc}


## Hilfe und Unterstützung anfordern
{: #getting-help-and-support}



Falls Sie Hilfe benötigen, können Sie in einem Forum nach Informationen suchen oder Fragen stellen. Sie können aber auch ein Support-Ticket öffnen. Wenn Sie eine Frage in einem Forum stellen, sollten Sie Ihre Frage mit einem Tag kennzeichnen, so dass sie von den {{site.data.keyword.cloud_notm}}-Entwicklerteams zur Kenntnis genommen wird.
{: shortdesc}

  * Bei technischen Fragen zu {{site.data.keyword.security-advisor_short}} sollten Sie Ihre Frage in [Stack Overflow](https://stackoverflow.com/){: external} posten. Dabei sollten Sie unbedingt die Tags `security-advisor` und `ibm-cloud` angeben.

  * Wenn Sie Fragen zum Service haben und Anweisungen zur Einführung benötigen, verwenden Sie das Forum [dW Answers](https://developer.ibm.com/){: external}. Dabei sollten Sie unbedingt die Tags `security-advisor` und `ibm-cloud` angeben.


Weitere Informationen zum Anfordern von Unterstützung finden Sie unter [Benötigte Unterstützung anfordern](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).


## Error: namespaces "security-advisor-insights" is forbidden
{: #ts-error-helm-install}

{: tsSymptoms}
Bei dem Versuch, Network Insights oder Activity Insights zu installieren, tritt der folgende Fehler auf:

```
namespaces "security-advisor-insights" is forbidden: User "system:serviceaccount:kube-system:default" cannot get resource "namespaces" in API group "" in the namespace "security-advisor-insights"
```
{: screen}

{: tsCauses}
Das Standardservicekonto `kube-system` verfügt in Ihrem Cluster nicht über Administratorzugriff.

{: tsResolve}
Bevor Sie eines der integrierten Insights-Angebote installieren können, müssen Sie die Helm-Komponente installieren. Beziehen Sie sich bei der Installation von Helm auf die [Dokumente zur Integration des Kubernetes-Service](/docs/containers?topic=containers-helm).


## Bekanntes Problem: Vorkommen einer angepassten Lösung kann nicht erstellt werden
{: #ts-custom-occurrence}

{: tsSymptoms}
Sie versuchen, ein Vorkommen einer angepassten Lösung zu erstellen, aber die Informationen werden in einem Browser nicht angezeigt und Sie erhalten keine Fehlernachricht.

{: tsCauses}
Das Erstellen des Vorkommens ist fehlgeschlagen, weil der von Ihnen gewählte Name bereits verwendet wird.

{: tsResolve}
Wählen Sie einen anderen Namen für das Vorkommen aus.

## Bekanntes Problem: Direktverbindungsimage wird nicht aktualisiert
{: #ts-known-cant-edit}

{: tsSymptoms}
Beim Hochladen oder Bearbeiten eines Image für eine Direktverbindung wird dieses nicht sofort angezeigt.

{: tsCauses}
Dass Images nicht korrekt angezeigt werden, ist ein bekanntes Problem.

{: tsResolve}
Um das aktualisierte Image anzuzeigen, müssen Sie Ihren Browser aktualisieren. Das neue Image wird in der Tabelle mit den Direktverbindungen angezeigt.

