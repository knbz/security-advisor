---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-13"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

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


# Partner
{: #setup-partner}

Le integrazioni Business Partner IBM sono un modo di utilizzare ricerche e avvisi critici da provider di terze parti nel dashboard {{site.data.keyword.security-advisor_long}}. Questi partner hanno collaborato con IBM per contribuire a creare, semplificare e guidare l'esperienza di integrazione al tuo posto.
{: shortdesc}

## Prima di cominciare
{: #partner-before}

Prima di iniziare l'integrazione con i partner, assicurati di avere i seguenti prerequisiti.

* Assicurati di disporre di un account con il partner che vuoi integrare.
* Assicurati di disporre delle autorizzazioni di amministrazione necessarie per generare l'URL di integrazione per il servizio partner.
* Assicurati di disporre dell'accesso di amministrazione IAM a {{site.data.keyword.cloud_notm}} e dell'accesso gestore a {{site.data.keyword.security-advisor_short}}.

## Procedura guidata di integrazione
{: #wizard}

In qualità di amministratore con diritti di gestione sia nell'account {{site.data.keyword.cloud_notm}} che negli account partner, puoi utilizzare la procedura guidata di integrazione per essere subito operativo. La procedura guidata ha quattro passi di base:

* Stabilire l'attendibilità e associare i tuoi account {{site.data.keyword.cloud_notm}} e partner
* Copiare le informazioni richieste, come le credenziali e gli URL, tra gli account
* Installare i metadati di ricerca del partner in {{site.data.keyword.security-advisor_short}}
* Convalidare l'accoppiamento pubblicando una ricerca dal partner in {{site.data.keyword.security-advisor_short}}


## Integrazione di NeuVector
{: #setup-neuvector}

Con NeuVector, puoi rilevare minacce e violazioni della rete per prevenire attacchi delle tue applicazioni basate su contenitore. Tramite il monitoraggio, puoi prevenire gli exploit e i breakout rilevando le escalation dei privilegi root, le scansioni delle porte, le shell inverse e l'attività sospetta del file system nei tuoi contenitori ed host.
{: shortdesc}

Per integrare NeuVector con {{site.data.keyword.security-advisor_short}}, puoi utilizzare la seguente procedura:

1. Passa a **Integrations > Partner Integrations** e seleziona **NeuVector** dalle opzioni fornite.
2. Connetti i tuoi account.
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



## Integrazione di Twistlock
{: #setup-twistlock}

Puoi evitare dei rischi arrestando lo sviluppo di immagini vulnerabili nel tuo ambiente. Con l'applicazione della politica personalizzata e automatizzata, Twistlock offre il controllo completo in ogni fase del ciclo di vita dell'applicazione.
{: shortdesc}

Quando configuri la connessione partner, vengono create due schede nel tuo dashboard che riepilogano le ricerche da Twistlock.

**Runtime Twistlock** introduce due KRI (Key Risk Indicator):

* Incidenti e controlli totali: ricerche correlate a incidenti o controlli sui tuoi carichi di lavoro nativi cloud.
* Controlli firewall totali: ricerche correlate a problemi con il tuo firewall.

**Vulnerabilità Twistlock** introduce un KRI:

* Immagini totali con nuove vulnerabilità: ricerche correlate alle vulnerabilità trovate nelle tue immagini contenitore.

Puoi trovare ulteriori informazioni sull'azienda nella documentazione di Twistlock.

### Configurazione di Twistlock
{: #configure-twistlock}

1. Nel dashboard {{site.data.keyword.security-advisor_short}}, passa a **Integrations > Partner Integrations** e seleziona **Twistlock** dalle opzioni fornite.

2. Fai clic su **Yes, connect my account to {{site.data.keyword.security-advisor_short}}**.

  Se non hai già un account, fai clic su **No, help me create an account > Create an account**. Puoi lasciare le tue informazioni e il team delle vendite di Twistlock ti contatterà per iniziare.
  {: note}

3. Fornisci un nome alla tua connessione. Assicurati che il nome sia univoco per il tuo account e di utilizzare solo caratteri alfanumerici, spazi o un trattino.

4. Facoltativo: se non hai ancora un URL di configurazione, fai clic su **Generate registration URL** per passare alla documentazione Twistlock e trovare le informazioni su come creare l'URL. Assicurati di avere il token Twistlock fornito con la tua licenza per ottenere l'accesso alla documentazione.

5. Nel dashboard Twistlock, passa alla scheda **Manage > Alerts** e fai clic su **Add profile**.

6. Seleziona **{{site.data.keyword.security-advisor_short}}** dall'elenco a discesa **Provider**.

7. Fai clic su **Copy** per utilizzare l'URL di configurazione fornito.

8. Torna al dashboard {{site.data.keyword.security-advisor_short}} e incolla l'URL nella casella **Enter the Twistlock configuration URL**.

9. Fai clic su **Connect Partner**.

### Verifica della connessione
{: #twistlock-verify}

1. Nel dashboard {{site.data.keyword.security-advisor_short}}, controlla se le schede Twistlock vengono visualizzate come previsto.

2. Nel dashboard Twistlock, aggiorna la scheda **Alerts**. Sarà visualizzata la connessione a {{site.data.keyword.security-advisor_short}}. Apri la connessione.

3. Verifica che i tipi di avviso da cui vuoi ricevere delle notifiche siano selezionati e poi fai clic su **Verify** per assicurarti che la connessione sia completa.
