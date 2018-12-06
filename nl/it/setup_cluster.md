---

copyright:
  years: 2018
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

# Configurazione del monitoraggio dei client e degli IP server sospetti per un cluster Kubernetes
{: #cluster_install}

Per provare la funzione di Analisi di rete, installa i componenti {{site.data.keyword.security-advisor_short}} in un cluster distribuito a {{site.data.keyword.containerlong_notm}}. Successivamente, puoi visualizzare informazioni approfondite e avvisi nel dashboard {{site.data.keyword.security-advisor_short}}.
{:shortdesc}

Questa funzione di {{site.data.keyword.security-advisor_short}} è un'anteprima tecnologica.
{: tip}

Vuoi saperne di più sull'Analisi di rete in {{site.data.keyword.security-advisor_short}}? [Controlla la documentazione](network-analytics.html).


## Prerequisiti
{: #cluster_prereqs}

Prima di cominciare: 

* Workstation dello sviluppatore Mac, Linux o Windows 10 
  * Windows 10: [abilita la funzione Linux Subsystem](https://win10faq.com/install-run-ubuntu-bash-windows-10/)
* [Node.js](https://nodejs.org/en/) 6 o superiore
* [jQ](https://stedolan.github.io/jq/download/)
* [Interfaccia riga di comando {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started) v0.6.5 o superiore
* [Plugin CLI {{site.data.keyword.containerlong_notm}}](https://console.bluemix.net/docs/containers/cs_cli_install.html#cs_cli_install)
* [CLI Kubernetes (kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.7 o superiore
* [Kubernetes Helm (gestore del pacchetto)](https://docs.helm.sh/using_helm/#from-script)
* [Un cluster Kubernetes standard o gratuito](https://console.bluemix.net/containers-kubernetes/catalog/cluster) nella regione **Stati Uniti Sud** di {{site.data.keyword.Bluemix_notm}}
* Identifica il proprietario dell'account {{site.data.keyword.Bluemix_notm}} per completare la procedura di installazione

## Accedi al tuo cluster
{: #login}

1.  Accedi a {{site.data.keyword.Bluemix_notm}}.

    ```
    ibmcloud login -a https://api.ng.bluemix.net --sso
    ```
    {: pre}

2.  Elenca tutti i cluster nell'account per ottenere il nome del cluster.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

3.  Indirizza la tua CLI al cluster. 

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

4.  Imposta il percorso al file di configurazione di Kubernetes locale come una variabile di ambiente. Esempio:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: pre}

5.  Configura Helm nel cluster.

    ```
    helm init
    ```
    {: pre}

Mantieni questa finestra della riga di comando aperta e continua.
{: tip}

## Installazione dei componenti di Security Advisor
{: #cluster_components}

Il programma di installazione deve essere eseguito dal proprietario dell'account {{site.data.keyword.Bluemix_notm}}.
{: tip}

Dopo aver configurato {{site.data.keyword.security-advisor_short}}, si verificano le seguenti azioni:
1. I metadati del cluster vengono raccolti e utilizzati per completare l'installazione degli indirizzi IP del nodo di lavoro, delle sottoreti, dell'indirizzo IP e dell'URL Ingress, del CRN del cluster, dell'endpoint di Analisi log, dello spazio e del token.
2. Vengono generati un ID servizio IAM e una chiave API nel tuo account {{site.data.keyword.Bluemix_notm}} in modo che il cluster possa essere identificato da Security Advisor. Se hai più di un cluster, esegui l'installazione per ogni cluster per generare gli ID servizio e le chiavi API di ogni cluster.
3. I componenti di {{site.data.keyword.security-advisor_short}} vengono distribuiti nello spazio dei nomi **monitoring** nel tuo cluster.

<br/>
Per installare i componenti di {{site.data.keyword.security-advisor_short}}:

1.  Scarica ed estrai il [pacchetto di installazione](https://github.com/IBM-Bluemix-Docs/security-advisor/blob/master/installation.tar.gz?raw=true).
2.  Dalla stessa finestra della riga di comando della sezione precedente, passa all'ubicazione dell'archivio estratto e installa il pacchetto.

    ```
    ./install.sh
    ```
    {: pre}

3.  Verifica che i componenti siano installati correttamente controllando il [dashboard {{site.data.keyword.security-advisor_short}}](https://console.bluemix.net/security-advisor/#/dashboard).

## Rimozione dei componenti di {{site.data.keyword.security-advisor_short}}
{: #cluster_uninstall}

Rimuovi i componenti di {{site.data.keyword.security-advisor_short}} dal tuo cluster e l'ID servizio e la chiave API dal tuo account {{site.data.keyword.Bluemix_notm}}.

1.  Accedi a {{site.data.keyword.Bluemix_notm}}.

    ```
    ibmcloud login -a https://api.ng.bluemix.net --sso
    ```
    {: pre}

2.  Elenca tutti i cluster nell'account per ottenere il nome del cluster.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

3.  Indirizza la tua CLI al cluster. 

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

4.  Imposta il percorso al file di configurazione di Kubernetes locale come una variabile di ambiente. Esempio:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: pre}

5.  Passa all'ubicazione dell'archivio estratto ed esegui lo script del programma di disinstallazione.

    ```
    ./uninstall.sh
    ```
    {: pre}

6.  Facoltativo: disinstalla il componente server Helm dal cluster.

    ```
    helm reset
    ```
    {: pre}
