---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

---

{:new_window: target="_blank"}
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

# Utilizzo vantaggioso dei servizi pre-integrati
{: #setup-services}

{{site.data.keyword.security-advisor_short}} viene fornito con numerose schede pre-popolate che possono aiutarti a monitorare i rischi e le minacce alla sicurezza.
{: shortdesc}

I seguenti servizi {{site.data.keyword.security-advisor_short}} creano automaticamente una scheda per:

* {{site.data.keyword.registrylong_notm}}
* {{site.data.keyword.cloudcerts_long_notm}}

Anche se non hai bisogno di fare nulla per creare la connessione tra {{site.data.keyword.security-advisor_short}} e i servizi, devi disporre di istanze dei servizi configurati con le informazioni.


## Monitoraggio delle vulnerabilità nelle immagini contenitore
{: #setup-images}

Con {{site.data.keyword.registryshort_notm}}, hai accesso a Vulnerability Advisor, che esegue la scansione continua delle immagini nella tua istanza {{site.data.keyword.registryshort_notm}} per rilevare potenziali problemi di sicurezza. Se vengono trovati dei problemi, vieni avvisato e puoi visualizzare un report completo nel tuo dashboard {{site.data.keyword.security-advisor_short}}.
{:shortdesc}

Ulteriori informazioni su [{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started).


### Prima di cominciare
{: #setup-before}

Prima di cominciare ad utilizzare il registro, assicurati di avere i seguenti plugin e CLI installati:
* [La CLI {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli)
* Il plugin Container Registry.

  ```
  ibmcloud plugin install container-registry
  ```
  {: codeblock}


### Creazione di uno spazio dei nomi
{: #setup-create-namespace}

1. Accedi al tuo account utilizzando la CLI.

  ```
  ibmcloud login --sso
  ```
  {: codeblock}

2. Accedi a {{site.data.keyword.registryshort_notm}}.

  ```
  ibmcloud cr login
  ```
  {: codeblock}

3. Facoltativo: crea uno spazio dei nomi. Puoi sempre utilizzarne uno esistente.

  ```
  ibmcloud cr namespace-add
  ```
  {: codeblock}

3. Contrassegna con tag un'immagine.

  ```
  docker tag <image>:<tag> <region>.icr.io/<namespace>/<image>:<tag>
  ```
  {: codeblock}

5. Esegui il push dell'immagine.

  ```
  docker push <region>.icr.io/<namespace>/<image>:<tag>
  ```
  {: codeblock}


Dopo aver eseguito il push dell'immagine al tuo spazio dei nomi {{site.data.keyword.registryshort_notm}}, vengono mostrate le informazioni su tutte le vulnerabilità rilevate nella scheda **Images with Vulnerabilities** nel dashboard del servizio. Puoi anche eseguire il drill-down di immagini specifiche per ottenere ulteriori informazioni, ad esempio una descrizione di tutte le vulnerabilità e dei problemi di configurazione identificati.


## Monitoraggio dei certificati
{: #setup-certificates}

Sapevi che {{site.data.keyword.cloudcerts_long_notm}} può essere utile per monitorare e gestire i tuoi certificati SSL/TLS? Integrando {{site.data.keyword.cloudcerts_short}} e {{site.data.keyword.security-advisor_short}}, puoi ricevere avvisi in anticipo sulla scadenza dei certificati che possono aiutare a prevenire un'interruzione del servizio o dell'applicazione.
{:shortdesc}

A seconda della data di scadenza del certificato che carichi in {{site.data.keyword.cloudcerts_short}}, le ricerche vengono visualizzate nel dashboard {{site.data.keyword.security-advisor_short}} 90, 60, 10 e 1 giorno prima della scadenza del certificato. Inoltre, ricevi notifiche giornaliere sui certificati scaduti. Le notifiche giornaliere iniziano il giorno successivo alla scadenza del tuo certificato.

Per attivare un aggiornamento manuale, potresti provare a caricare un certificato che scade in un giorno nella tua istanza {{site.data.keyword.cloudcerts_short}}. Al termine dell'importazione, puoi vedere che il KRI (Key Risk Indicator) e le ricerche sono visibili nel dashboard {{site.data.keyword.security-advisor_short}}.

Puoi trovare ulteriori informazioni su [{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager?topic=certificate-manager-getting-started) consultando la documentazione.
{: tip}

### Creazione di un certificato
{: #setup-create-cert}

Per creare un certificato autofirmato che scade in un giorno, puoi eseguire il seguente comando OpenSSL nel tuo terminale.

```
openssl req -x509 -newkey rsa:2048 -keyout key.pem -subj "/CN=myservice.com" -out server.pem -days 1 -nodes
```
{: codeblock}


### Caricamento di un certificato
{: #setup-upload-cert}

1. Nel catalogo {{site.data.keyword.cloud_notm}}, cerca "{{site.data.keyword.cloudcerts_short}}".
2. Fornisci alla tua istanza del servizio un nome o utilizza il nome preimpostato.
3. Fai clic su **Create**.
4. Per importare i certificati della tua azienda in {{site.data.keyword.cloudcerts_short}}, fai clic su **Import Certificate**.

Ora che i tuoi certificati sono stati importati, le informazioni come le date di scadenza e i certificati scaduti, vengono mostrate nella scheda **Certificates** nel dashboard {{site.data.keyword.security-advisor_short}}. Facendo clic sulla scheda, puoi ottenere informazioni più specifiche sui certificati, come ad esempio l'istanza del servizio a cui appartengono i certificati. Puoi anche vedere tutti i passi che puoi eseguire per correggere le vulnerabilità di sicurezza.

Per interrompere le notifiche, devi rinnovare il tuo certificato, caricare il certificato in {{site.data.keyword.cloudcerts_short}} ed eliminare il certificato scaduto.
{: tip}
