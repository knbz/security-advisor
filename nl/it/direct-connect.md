---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

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


# Connessioni dirette
{: #setup-custom-gui}

Con {{site.data.keyword.security-advisor_short}}, puoi integrare i tuoi strumenti di sicurezza personalizzati esistenti, sia che siano servizi open source, sviluppati in modo personalizzato o di terze parti. Puoi creare una connessione diretta da {{site.data.keyword.security-advisor_short}} a un altro strumento aggiungendo l'URL al tuo elenco di integrazioni. Creando il link, puoi monitorare facilmente tutti gli strumenti che utilizzi.
{: shortdesc}


## Prima di cominciare
{: #custom-before-gui}

Prima di poter aggiungere l'integrazione, devi prima avere un account con il partner che vuoi integrare.

{{site.data.keyword.security-advisor_short}} non salva in modo permanente alcuna credenziale correlata al servizio partner. Gli utenti aziendali devono autenticarsi utilizzando SAML sia per {{site.data.keyword.cloud_notm}} che per il business partner.
{: note}

## Configurazione della connessione
{: #custom-configure-connection}

1. Accedi al tuo strumento di sicurezza e ottieni il tuo URL univoco.

2. Accedi a {{site.data.keyword.cloud_notm}} utilizzando la console.

3. Fai clic su **Custom Integrations > Direct Connection**. Viene visualizzata una schermata.

  1. Fornisci un nome alla tua soluzione. Nel nome puoi utilizzare solo caratteri alfanumerici, spazi vuoti e trattini (-).

  2. Immetti l'URL per la soluzione nel formato: `www.<website>.<domain>`.

  3. Carica un'icona o un'immagine per rappresentare lo strumento.

  4. Fai clic su **Connect** per completare la configurazione. {{site.data.keyword.security-advisor_short}} crea le risorse richieste per l'integrazione come l'ID servizio, la chiave API, l'ID account e i metadati. Viene assegnato il ruolo `writer`.
