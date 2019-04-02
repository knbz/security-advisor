---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-13"

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

# Network Insights
{: #setup-network}

Con {{site.data.keyword.security-advisor_long}}, puoi monitorare il comportamento utilizzando il machine learning, i modelli comportamentali e il threat intelligence per rilevare ii contenitori potenzialmente compromessi eseguiti sui tuoi cluster {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

A partire dal 20 gennaio 2019, Network Insights (beta) sostituisce la funzione Network Analytics. Tutte le schede di analisi nel tuo dashboard del servizio vengono eliminate, ma le ricerche rimangono nel database delle ricerche.
{: important}

## Prima di cominciare
{: #network-prereq}

Se al momento stai utilizzando la funzione Network Analytics, devi [eliminare i componenti del servizio](/docs/services/security-advisor?topic=security-advisor-setup-network#uninstall-analytics) prima di poter installare Network Insights. Per iniziare ad utilizzare Network Insights, assicurati di avere i seguenti prerequisiti.

- Se stai utilizzando Windows 10, attiva il sottosistema Windows per Linux e installa una [shell Ubuntu](https://win10faq.com/install-run-ubuntu-bash-windows-10/)
- Installa la CLI yq:
  * Per [macOS e Windows 10](http://mikefarah.github.io/yq/).
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
- La [CLI Kubernetes](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.10.11 o superiore
- [Kubernetes Helm (gestore del pacchetto)](/docs/containers?topic=containers-integrations#helm) v2.9.0 o superiore.
- Un cluster Kubernetes standard versione v1.10.11 o superiore

## Creazione di un bucket COS
{: #network-setup-cos}

Utilizzando la GUID {{site.data.keyword.security-advisor_short}}, puoi creare un nuovo bucket e una nuova istanza COS.

1. Sulla scheda **Integrations** del dashboard del servizio, cambia **Analysis Disabled** nella casella Network Insights con **Analysis Enabled**.

2. Fai clic su **Go to set up**.

3. Nella sezione dei prerequisiti, fai clic su **Create COS instance and bucket**. Il tuo bucket e la tua istanza COS vengono automaticamente creati per tuo conto con la convenzione di denominazione corretta e le autorizzazioni IAM. 

Se hai un'istanza esistente di COS e un bucket, assicurati che venga utilizzata la convenzione di denominazione: `sa.<account_id>.telemetric.<cos_region>`. Per consentire al servizio di leggere i dati archiviati nella tua istanza COS, configura l'[autorizzazione da-servizio-a-servizio](/docs/iam?topic=iam-serviceauth#serviceauth) utilizzando {{site.data.keyword.cloud_notm}} IAM. Imposta `source` su `{{site.data.keyword.security-advisor_short}}` e `target` sulla tua istanza COS. Assegna il ruolo IAM `Reader`.

## Installazione dei componenti {{site.data.keyword.security-advisor_short}}
{: #network-install-components}

Puoi installare un agent per raccogliere i log del flusso di rete dal tuo cluster Kubernetes. I log vengono archiviati nella tua istanza Cloud Object Storage. Puoi quindi abilitare Network Insights per analizzare i tuoi log e rilevare e avvisarti di attività di rete sospetta.
{: shortdesc}

Assicurati di ripetere l'installazione di ogni cluster che vuoi monitorare.
{: note}

1. Clona il seguente repository nel tuo sistema locale.

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-network-insights.git
  ```
  {: codeblock}

2. Passa alla cartella `security-advisor-network-analytics`.

3. Estrai il file `.tar` immettendo il seguente comando.

  ```
  tar -xvf security-advisor-network-insights.tar
  ```
  {: codeblock}

4. Passa alla cartella `security-advisor-network-insights`.

5. Accedi alla CLI {{site.data.keyword.cloud_notm}}. Segui le richieste nella CLI per completare l'accesso.

  ```
  ibmcloud login -a https://api.<region>.bluemix.net
  ```
  {: codeblock}

  <table>
    <tr>
      <th>Regione </th>
      <th>Endpoint</th>
    </tr>
    <tr>
      <td>Regno Unito </td>
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

7. Installa Helm utilizzando la [documentazione di integrazione di Kubernetes Service](/docs/containers?topic=containers-integrations#helm).

8. Facoltativo: [Abilita TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md). Se stai utilizzando la tua workstation per gestire l'installazione dei componenti di analisi in più cluster e TLS è abilitato, assicurati che le configurazioni TLS siano correnti e corrispondano al cluster corrente in cui pianifichi di installare i componenti.

9. Immetti il seguente comando per installare il grafico Helm e le relative dipendenze. Il comando convalida che il tuo bucket utilizza la convenzione di denominazione corretta, crea i segreti Kubernetes, aggiorna i valori con il tuo GUID del cluster e distribuisce il grafico Helm di Network Insights. Se riscontri un errore, prova ad eseguire `helm init --upgrade`.
  

  ```
  ./network-insight-install.sh <cos_region> <cos_api_key>
  ```
  {: codeblock}

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
      <td>La [chiave API](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials#service-credentials) che hai creato per accedere al tuo bucket e alla tua istanza COS. La chiave deve avere il ruolo della piattaforma `writer`.</td>
    </tr>
  </table>

Hai correttamente configurato il tuo cluster per utilizzare {{site.data.keyword.security-advisor_short}} Network Insights!



## Eliminazione dei componenti
{: #network-delete}

Se non hai più bisogno di utilizzare Network Insights, puoi eliminare i componenti del servizio dal tuo cluster.
{: shortdesc}

1. Accedi alla CLI {{site.data.keyword.cloud_notm}}. Segui le richieste nella CLI per completare l'accesso.

  ```
  ibmcloud login -a https://api.<region>.bluemix.net
  ```
  {: codeblock}

  <table>
    <tr>
      <th>Regione </th>
      <th>Endpoint</th>
    </tr>
    <tr>
      <td>Regno Unito </td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>Stati Uniti Sud</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

2. Imposta il contesto per il tuo cluster.

  1. Richiama il comando per impostare la variabile di ambiente e scaricare i file di configurazione Kubernetes.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. Copia l'output che inizia con `export` e incollalo nel tuo terminale per impostare la variabile di ambiente `KUBECONFIG`.

3. Elimina i componenti utilizzando Helm. 

  ```
  helm del --purge network-insights [--tls]
  ```
  {: codeblock}

4. Elimina i segreti di Kubernetes.

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}

Assicurati di eliminare il processo per ogni cluster da cui vuoi rimuovere gli agent.
{: tip}

## Disinstallazione di Network Analytics
{: #uninstall-analytics}

Se hai utilizzato la versione beta di Network Analytics, devi disinstallare i precedenti componenti di {{site.data.keyword.security-advisor_short}} prima di poter installare i nuovi componenti. Assicurati di ripetere questo processo per ogni cluster che contiene dei componenti del servizio.
{: shortdesc}

1. Accedi a {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud login -a https://api.us-south.ibm.cloud.com --sso
  ```
  {: pre}

2. Elenca tutti i cluster nel tuo account per ottenere il nome del cluster.

  ```
  ibmcloud ks clusters
  ```
  {: pre}

3. Indirizza la tua CLI al cluster.

  ```
  ibmcloud ks cluster-config <cluster-name-or-id>
  ```
  {: pre}

4. Imposta il percorso al file di configurazione di Kubernetes locale come una variabile di ambiente. Esempio:

  ```
  export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
  ```
  {: pre}

5. Passa all'ubicazione dell'archivio estratto ed esegui lo script del programma di disinstallazione.

  ```
  ./uninstall.sh
  ```
  {: pre}

6. Facoltativo: disinstalla il componente server Helm dal cluster.

  ```
  helm reset
  ```
  {: pre}
