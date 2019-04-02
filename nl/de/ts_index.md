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

# Fehlerbehebung
{: #troubleshooting}

Treten bei der Arbeit mit {{site.data.keyword.security-advisor_long}} Probleme auf, lesen Sie die folgenden Informationen zur Fehlerbehebung und zum Anfordern von Hilfe.
{: shortdesc}


## Hilfe und Unterstützung anfordern
{: #getting-help-and-support}



Falls Sie Hilfe benötigen, können Sie in einem Forum nach Informationen suchen oder Fragen stellen. Sie können aber auch ein Support-Ticket öffnen. Wenn Sie eine Frage in einem Forum stellen, sollten Sie Ihre Frage mit einem Tag kennzeichnen, so dass sie von den {{site.data.keyword.Bluemix_notm}}-Entwicklerteams zur Kenntnis genommen wird.
{: shortdesc}

* Bei technischen Fragen zu {{site.data.keyword.security-advisor_short}} sollten Sie Ihre Frage in <a href="http://stackoverflow.com/" target="_blank">Stack Overflow <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> posten. Dabei sollten Sie unbedingt die Tags `security-advisor` und `ibm-cloud` angeben.
* Wenn Sie Fragen zum Service haben und Anweisungen zur Einführung benötigen, verwenden Sie das Forum <a href="https://developer.ibm.com/" target="_blank">dW Answers <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. Dabei sollten Sie unbedingt die Tags `security-advisor` und `ibm-cloud` angeben.

Weitere Informationen zum Anfordern von Unterstützung finden Sie unter [Benötigte Unterstützung anfordern](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).


## Vorkommen einer angepassten Lösung kann nicht erstellt werden
{: #ts-custom-occurrence}

{: tsSymptoms}
Sie versuchen, ein Vorkommen einer angepassten Lösung zu erstellen, aber die Informationen werden in einem Browser nicht angezeigt und Sie erhalten keine Fehlernachricht.

{: tsCauses}
Es ist ein bekanntes Problem aufgetreten. Das Erstellen des Vorkommens ist fehlgeschlagen, weil der von Ihnen gewählte Name bereits vorhanden ist.

{: tsResolve}
Wählen Sie einen anderen Namen für das Vorkommen aus.


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
Bevor Sie eines der integrierten Insights-Angebote installieren können, müssen Sie die Helm-Komponente installieren. Beziehen Sie sich bei der Installation von Helm auf die [Dokumente zur Integration des Kubernetes-Service](/docs/containers?topic=containers-integrations#helm).


## Bekannter Fehler: Manche Ergebnisse von Network Insights werden nicht angezeigt
{: #ts-network-analytics}

{: tsSymptoms}
Wenn Sie über das Dashboard oder die API nach Untersuchungsergebnissen von Network Insights suchen, werden nicht alle Untersuchungsergebnistypen angezeigt.

{: tsCauses}
Manche Network Insights-Ergebnisse funktionieren nur in Verbindung mit Version 1.10 von IBM Cloud Kubernetes Service Version oder älter.

{: tsResolve}
Verwenden Sie Kubernetes Service in der Version 1.10 oder älter.
