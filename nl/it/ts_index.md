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

# Risoluzione dei problemi
{: #troubleshooting}

Se hai dei problemi quando utilizzi {{site.data.keyword.security-advisor_long}}, tieni presenti queste tecniche per la risoluzione dei problemi e su come ottenere supporto.
{: shortdesc}


## Come ottenere aiuto e supporto
{: #getting-help-and-support}



Puoi trovare supporto ricercando informazioni o facendo una domanda in un forum. Puoi inoltre aprire un ticket di supporto. Quando utilizzi i forum per fare una domanda, contrassegnala con una tag in modo che sia visualizzabile dai team di sviluppo {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

* Se hai domande tecniche su {{site.data.keyword.security-advisor_short}}, inserisci la tua domanda in <a href="http://stackoverflow.com/" target="_blank">Stack Overflow <img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a>. Assicurati di includere le tag `security-advisor` e `ibm-cloud`.
* Per domande sul servizio e sulle istruzioni per l'utilizzo iniziale, utilizza il forum <a href="https://developer.ibm.com/" target="_blank">dW Answers <img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a>. Assicurati di includere le tag `security-advisor` e `ibm-cloud`.

Per ulteriori informazioni su come ottenere supporto, consulta [come posso ottenere il supporto di cui ho bisogno?](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).


## Non puoi creare una ricorrenza della soluzione personalizzata
{: #ts-custom-occurrence}

{: tsSymptoms}
Tenti di creare una ricorrenza della soluzione personalizzata ma le informazioni non vengono mostrate in un browser e non ricevi alcun messaggio di errore.

{: tsCauses}
Hai riscontrato un problema noto. La creazione della ricorrenza non è riuscita perché il nome scelto esiste già.

{: tsResolve}
Scegli un altro nome per la tua ricorrenza.


## Errore: gli spazi dei nomi "security-advisor-insights" non sono consentiti
{: #ts-error-helm-install}

{: tsSymptoms}
Quando tenti di installare Network o Activity Insights, riscontri l'errore:

```
namespaces “security-advisor-insights” is forbidden: User “system:serviceaccount:kube-system:default” cannot get resource “namespaces” in API group “” in the namespace “security-advisor-insights”
```
{: screen}

{: tsCauses}
L'account del servizio predefinito `kube-system` non dispone dell'accesso di amministratore nel tuo cluster.

{: tsResolve}
Prima di installare una delle offerte Built-in Insights, devi installare Helm. Puoi installare Helm utilizzando la [documentazione di integrazione di Kubernetes Service](/docs/containers?topic=containers-integrations#helm).


## Problema noto: alcune ricerche di Network Insights non vengono mostrate
{: #ts-network-analytics}

{: tsSymptoms}
Non vedi tutti i tipi di ricerca quando esegui una ricerca Network Insights tramite il dashboard o l'API.

{: tsCauses}
Alcuni tipi di ricerca di Network Insights funzionano solo su IBM Cloud Kubernetes Service versione 1.10 o inferiore.

{: tsResolve}
Utilizza Kubernetes Service versione 1.10 o inferiore.
