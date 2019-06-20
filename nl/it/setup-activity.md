---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

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

# Activity Insights
{: #setup-activity}

Con {{site.data.keyword.security-advisor_long}}, puoi monitorare in modo continuo i tuoi log del Programma di traccia dell'attività {{site.data.keyword.cloud_notm}} per identificare delle modifiche e dei comportamenti non autorizzati o sospetti nelle tue risorse. Puoi utilizzare i pacchetti di regole basati sulle procedure consigliate fornite dal servizio o creare le tue regole personalizzate.
{: shortdesc}


## Prima di cominciare
{: #activity-prereq}

Per iniziare ad utilizzare Activity Insights, assicurati di avere i seguenti prerequisiti.

- Se stai utilizzando Windows 10, attiva il sottosistema Windows per Linux e installa una [shell Ubuntu](https://win10faq.com/install-run-ubuntu-bash-windows-10/){: external}.
- Installa la CLI yq:
  * Per [macOS e Windows 10](http://mikefarah.github.io/yq/){: external}.
  * Per CentOS, Red Hat e Ubuntu immetti i seguenti comandi per installare la versione 1.15.
    ```
    wget https://github.com/mikefarah/yq/releases/download/1.15.0/yq_linux_amd64       
    mv yq_linux_amd64 yq   
    chmod +x yq    
    mv yq /usr/local/bin/     
    yq -V
    ```
    {: codeblock}     
- File binario cURL aggiornato: per CentOS e Red Hat, puoi eseguire l'aggiornamento eseguendo `yum update -y nss curl libcurl`.
- La CLI [{{site.data.keyword.cloud_notm}} e i plugin richiesti](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)
- La [CLI Kubernetes](https://kubernetes.io/docs/tasks/tools/install-kubectl/){: external} v1.10.11 o superiore
- [Kubernetes Helm (gestore del pacchetto)](/docs/containers?topic=containers-helm) v2.9.0 o superiore.
- Un cluster Kubernetes standard versione v1.10.11 o superiore


## Creazione di un bucket COS
{: #activity-setup-cos}

Utilizzando la GUID {{site.data.keyword.security-advisor_short}}, puoi creare un nuovo bucket e una nuova istanza COS.

1. Passa alla scheda **Integrations** del dashboard del servizio.

2. Modifica **Analysis Disabled** nella casella Activity Insights con **Analysis Enabled**.

3. Fai clic su **Go to set up**.

4. Nella sezione dei prerequisiti, fai clic su **Create COS instance and bucket**. Il tuo bucket e la tua istanza COS vengono automaticamente creati per tuo conto con la convenzione di denominazione corretta e le autorizzazioni IAM. Vengono visualizzate le informazioni sul bucket.

Se hai un'istanza esistente di COS e un bucket, assicurati che venga utilizzata la convenzione di denominazione `sa.<account_id>.telemetric.<cos_region>`. Per consentire al servizio di leggere i dati archiviati nella tua istanza COS, configura l'[autorizzazione da-servizio-a-servizio](/docs/iam?topic=iam-serviceauth) utilizzando {{site.data.keyword.cloud_notm}} IAM. Imposta `source` su `{{site.data.keyword.security-advisor_short}}` e `target` sulla tua istanza COS. Assegna il ruolo IAM `Reader`.


## Installazione dei componenti {{site.data.keyword.security-advisor_short}}
{: #activity-install-components}

Puoi installare un agent per raccogliere i log del flusso di controllo dal tuo account {{site.data.keyword.cloud_notm}}. I log vengono archiviati nella tua istanza Cloud Object Storage dove puoi abilitare Activity Insights per analizzare i tuoi log alla ricerca di un comportamento sospetto.
{: shortdesc}

1. Clona il seguente repository nel tuo sistema locale.

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. Passa alla cartella `security-advisor-activity-insights`.

3. Estrai il file `.tar` immettendo il seguente comando.

  ```
  tar -xvf security-advisor-activity-insights.tar
  ```
  {: codeblock}

4. Passa alla cartella `security-advisor-activity-insights`.
5. Accedi alla CLI {{site.data.keyword.cloud_notm}}. Segui le richieste nella CLI per completare l'accesso.

  ```
  ibmcloud login -a cloud.ibm.com -r <region>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>Regione</th>
      <th>Endpoint</th>
    </tr>
    <tr>
      <td>Regno Unito</td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>Stati Uniti Sud</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

6. Imposta il contesto per il tuo cluster.

  1. Richiama il comando per impostare la variabile di ambiente e scaricare i file di configurazione Kubernetes.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. Copia l'output che inizia con `export` e incollalo nel tuo terminale per impostare la variabile di ambiente `KUBECONFIG`.

7. Installa Helm utilizzando la [documentazione di integrazione di Kubernetes Service](/docs/containers?topic=containers-helm).

8. Facoltativo: [Abilita TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md){: external}. Se stai utilizzando la tua workstation per gestire l'installazione dei componenti di analisi in più cluster e TLS è abilitato, assicurati che le configurazioni TLS siano correnti e corrispondano al cluster corrente in cui pianifichi di installare i componenti.

9. Immetti il seguente comando per installare Insights. Il comando convalida la convenzione di denominazione del tuo bucket, crea i segreti Kubernetes, aggiorna i valori con il tuo GUID del cluster e distribuisce Activity Insights.

  ```
  ./activity-insight-install.sh <cos_region> <cos_api_key> <at_region> <account_api_key> <account_spaces>
  ```
  {: codeblock}

  Se riscontri un errore, prova ad eseguire `helm init --upgrade`.
  {: tip}

  <table>
    <tr>
      <th>Variabile</th>
      <th>Descrizione</th>
    </tr>
    <tr>
      <td><code>cos_region</code></td>
      <td>La regione in cui viene distribuita la tua istanza COS. Le opzioni includono: <code>us-south</code> e <code>eu-gb</code>.</td>
    </tr>
    <tr>
      <td><code>cos_api_key</code></td>
      <td>La [chiave API](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) che hai creato per accedere al tuo bucket e alla tua istanza COS. La chiave deve avere il ruolo della piattaforma `writer`.</td>
    </tr>
    <tr>
      <td><code>at_region</code></td>
      <td>La regione in cui hai creato il tuo bucket e la tua istanza COS. Le opzioni includono: <code>us-south</code> e <code>eu-gb</code>.</td>
    </tr>
    <tr>
      <td><code>account_api_key</code></td>
      <td>La chiave API della piattaforma per il tuo account {{site.data.keyword.cloud_notm}}.</td>
    </tr>
    <tr>
      <td><code>account_spaces</code></td>
      <td>Un elenco separato da virgole di GUID dello spazio per il tuo account {{site.data.keyword.cloud_notm}}.</td>
    </tr>
  </table>


## Aggiunta dei pacchetti di regole a COS
{: #activity-adding-rules}

Un pacchetto di regole è un file JSON che contiene un elenco di regole che vuoi monitorare. Puoi scaricare i pacchetti di regole o [creare le tue regole](/docs/services/security-advisor?topic=security-advisor-activity#activity-packages). Il motore di {{site.data.keyword.security-advisor_short}} convalida che ogni regola segua la sintassi corretta.
{: shortdesc}

1. Clona il seguente repository per ottenere diversi pacchetti di regole preimpostati. Sul tuo sistema locale, viene creata una cartella con il nome `security-advisor-activity-insights`.

  ```
  https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. In locale, crea una cartella con il nome `IBM.rules/activities`.

3. Copia i file JSON da `security-advisor-activity-insights/security-advisor-ata-rule-packages` a `IBM.rules/activities`.

4. Passa al tuo dashboard {{site.data.keyword.cloud_notm}} e seleziona l'istanza del servizio COS associata a Activity Insights.

5. Sulla scheda **Buckets** del dashboard del servizio, seleziona il bucket associato a Activity Insights.

5. Nella home page dell'istanza COS, fai clic su **Upload** e seleziona **Folders**.

6. Se richiesto, fai clic su **Install Aspera Connect client**. Se non vedi una richiesta, il client è già installato. Se hai bisogno di installare il client, ripeti il passo 5 al termine dell'installazione.

7. Seleziona la cartella *IBM.rules/activities*.

8. Fai clic su **Upload**.

Vuoi utilizzare i tuoi pacchetti? Utilizza uno dei file JSON come una guida per creare delle regole che si adattano ai bisogni delle tue organizzazioni. Dopo aver creato il file, aggiungilo alla cartella *IBM.rules/activities* nella tua istanza COS. Per ulteriori informazioni sui tipi di regole e la loro formattazione, vedi [Descrizione dei pacchetti di regole](/docs/services/security-advisor?topic=security-advisor-activity).
{: tip}


## Eliminazione dei componenti
{: #activity-delete}

Se non hai più bisogno di utilizzare Activity Insights, puoi eliminare i componenti del servizio dal tuo cluster.
{: shortdesc}

1. Elimina i componenti del servizio utilizzando Helm. Assicurati di utilizzare l'indicatore `-tls` se TLS è abilitato.

  ```
  helm del --purge activity-insights [--tls]
  ```
  {: codeblock}

2. Elimina i segreti di Kubernetes.

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}
