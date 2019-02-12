---

copyright:
  years: 2018
lastupdated: "2018-12-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Soluzioni partner
{: #setup-partner}

Le soluzioni partner sono un modo per estendere la tua sicurezza creando un collegamento tra {{site.data.keyword.security-advisor_long}} e altri strumenti di sicurezza.
{: shortdesc}

## Procedura guidata di integrazione
{: #wizard}

In qualità di amministratore con diritti di gestione sia nell'account IBM Cloud che nell'account partner, puoi utilizzare la procedura guidata di integrazione per essere subito operativo. La procedura guidata ha quattro passi di base:

* Stabilire l'attendibilità e associare i tuoi account IBM Cloud e partner
* Copiare le informazioni richieste, come le credenziali e gli URL, tra gli account
* Installare i metadati di ricerca del partner in {{site.data.keyword.security-advisor_short}}
* Convalidare l'accoppiamento pubblicando una ricerca dal partner in {{site.data.keyword.security-advisor_short}}

</br>

## Integrazione di NeuVector
{: #neuvector}

**Prima di cominciare**

* Assicurati di disporre di un account con il partner che vuoi integrare.
* Assicurati di disporre delle autorizzazioni di amministrazione necessarie per generare l'URL di integrazione per il servizio partner.
* Assicurati di disporre dell'accesso di amministrazione IAM a IBM Cloud e dell'accesso gestore a {{site.data.keyword.security-advisor_short}}.

**Integrazione**

1. Connetti i tuoi account.
  1. Fornisci un nome per la tua connessione.
  2. Fornisci l'URL del dashboard NeuVector.
  3. Fornisci l'URL di configurazione di NeuVector.
    1. Accedi a NeuVector e vai alle impostazioni.
    2. Fai clic su **Generate URL**.
    3. Copia l'URL e incollalo nella procedura guidata di integrazione di {{site.data.keyword.security-advisor_short}}.
  4. Fai clic su **Next**.
3. Verifica di autorizzare il servizio a generare un ID servizio e una chiave API e creare la connessione con il servizio facendo clic **Next**.
4. Fai clic su **Done**.
5. Vai al tuo dashboard del servizio per esaminare una ricerca di prova inviata a {{site.data.keyword.containershort_notm}} da NeuVector.
