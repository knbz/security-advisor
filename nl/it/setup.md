---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Configurazione di {{site.data.keyword.security-advisor_short}}
{: #setup}

Con {{site.data.keyword.security-advisor_long}}, puoi monitorare continuamente le tue applicazioni per rilevare rischi e minacce di sicurezza. Quando vengono rilevate delle vulnerabilità, vieni avvisato tramite il dashboard del servizio.
{:shortdesc}

## Monitoraggio delle vulnerabilità nelle immagini contenitore
{: #setup_images}

Un'immagine Docker è la base di ogni contenitore che crei. L'immagine potrebbe contenere la tua applicazione, la sua configurazione e tutte le dipendenze necessarie. Un'immagine viene normalmente archiviata in un registro. Utilizzando {{site.data.keyword.registryshort_notm}}, hai accesso al Controllo vulnerabilità, che esegue continuamente la scansione delle tue immagini alla ricerca di problemi di sicurezza potenziali. Se vengono trovati dei problemi, vieni avvisato e puoi visualizzare un report completo nel tuo dashboard {{site.data.keyword.security-advisor_short}}.
{:shortdesc}

Ulteriori informazioni su [{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html#index). 


**Prima di cominciare**

Prima di cominciare ad utilizzare il registro, assicurati di avere i seguenti plugin e CLI installati:
- [La CLI {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://clis.ng.bluemix.net/ui/home.html)
- Il plugin container-registry.

    ```
    ibmcloud plugin install container-registry -r Bluemix
    ```
    {: pre}

</br>
**Creazione di uno spazio dei nomi**

1. Accedi al tuo account utilizzando la CLI.

   ```
   bx login --sso
   ```
   {: pre}

2. Accedi a {{site.data.keyword.registryshort_notm}}.

   ```
   bx cr login
   ```
   {: pre}

3. Facoltativo: crea uno spazio dei nomi. Puoi sempre utilizzarne uno esistente.

   ```
   bx cr namespace-add
   ```
   {: pre}

3. Contrassegna con tag un'immagine. 

   ```
   docker tag <image>:<tag> registry.ng.bluemix.net/<namespace>/<image>:<tag>
   ```
   {: pre}

5. Esegui il push dell'immagine.

   ```
   docker push registry.ng.bluemix.net/<namespace>/<image>:<tag>
   ```
   {: pre}


Dopo aver eseguito il push dell'immagine al tuo spazio dei nomi {{site.data.keyword.registryshort_notm}}, vengono mostrate le informazioni su tutte le vulnerabilità rilevate nella scheda **Images with Vulnerabilities** nel dashboard del servizio. Puoi anche eseguire il drill down in immagini specifiche per trovare ulteriori informazioni, come la descrizione di tutti i problemi di configurazione e le vulnerabilità noti che vengono identificati.

</br>

## Monitoraggio dei certificati
{: #setup_certificates}

Sapevi che {{site.data.keyword.cloudcerts_long_notm}} può essere utile per monitorare e gestire i tuoi certificati SSL/TLS? Con l'integrazione di {{site.data.keyword.cloudcerts_short}} e {{site.data.keyword.security-advisor_short}}, puoi richiamare avvisi e promemoria su quando potresti aver bisogno di aggiornare i tuoi certificati e altre informazioni, che possono aiutare a prevenire vulnerabilità future.
{:shortdesc}

Puoi trovare ulteriori informazioni su [{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager/index.html#gettingstarted).

1. Nel catalogo {{site.data.keyword.Bluemix_notm}}, cerca "{{site.data.keyword.cloudcerts_short}}".
2. Fornisci alla tua istanza del servizio un nome o utilizza il nome preimpostato.
3. Fai clic su **Create**.
4. Per importare i certificati della tua azienda in {{site.data.keyword.cloudcerts_short}}, fai clic su **Import Certificate**.

Ora che i tuoi certificati sono stati importati, le informazioni come le date di scadenza e i certificati scaduti, vengono mostrate nella scheda **Certificates** nel dashboard {{site.data.keyword.security-advisor_short}}. Facendo clic sulla scheda, puoi ottenere informazioni più specifiche sui certificati, come ad esempio l'istanza del servizio a cui appartengono i certificati. Puoi anche vedere tutti i passi che puoi eseguire per correggere le vulnerabilità di sicurezza.
