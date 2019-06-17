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

# Risoluzione dei problemi
{: #troubleshooting}

Se hai dei problemi quando utilizzi {{site.data.keyword.security-advisor_long}}, tieni presenti queste tecniche per la risoluzione dei problemi e su come ottenere supporto.
{: shortdesc}


## Come ottenere aiuto e supporto
{: #getting-help-and-support}



Puoi trovare supporto ricercando informazioni o facendo una domanda in un forum. Puoi inoltre aprire un ticket di supporto. Quando utilizzi i forum per fare una domanda, contrassegnala con una tag in modo che sia visualizzabile dai team di sviluppo {{site.data.keyword.cloud_notm}}.
{: shortdesc}

  * Se hai domande tecniche su {{site.data.keyword.security-advisor_short}}, inserisci la tua domanda in [Stack Overflow](https://stackoverflow.com/){: external}. Assicurati di includere le tag `security-advisor` e `ibm-cloud`.

  * Per domande sul servizio e sulle istruzioni per l'utilizzo iniziale, utilizza il forum [dW Answers](https://developer.ibm.com/){: external}. Assicurati di includere le tag `security-advisor` e `ibm-cloud`.


Per ulteriori informazioni su come ottenere supporto, consulta [come posso ottenere il supporto di cui ho bisogno?](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).


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
Prima di installare una delle offerte Built-in Insights, devi installare Helm. Puoi installare Helm utilizzando la [documentazione di integrazione di Kubernetes Service](/docs/containers?topic=containers-helm).


## Problema noto: non puoi creare una ricorrenza della soluzione personalizzata
{: #ts-custom-occurrence}

{: tsSymptoms}
Tenti di creare una ricorrenza della soluzione personalizzata ma le informazioni non vengono mostrate in un browser e non ricevi alcun messaggio di errore.

{: tsCauses}
La creazione della ricorrenza non è riuscita perché il nome che hai scelto è in uso.

{: tsResolve}
Scegli un altro nome per la tua ricorrenza.

## Problema noto: l'immagine della connessione diretta non si aggiorna
{: #ts-known-cant-edit}

{: tsSymptoms}
Quando carichi o modifichi un'immagine per una connessione diretta, questa non viene immediatamente visualizzata.

{: tsCauses}
È un problema noto che le immagini non vengono visualizzate correttamente.

{: tsResolve}
Per vedere l'immagine aggiornata, aggiorna il tuo browser. La nuova immagine viene visualizzata nella tabella di connessione diretta.

