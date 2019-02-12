---

copyright:
  years: 2018
lastupdated: "2018-12-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:note: .note}

# Configurazione di Analisi di rete (beta)
{: #cluster_install}

Puoi provare la funzione di analisi di rete del servizio installando {{site.data.keyword.security-advisor_short}} in un cluster {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

L'analisi di rete è una funzione di anteprima che è disponibile solo nella regione Stati Uniti Sud.
{: note}

Quando configuri {{site.data.keyword.security-advisor_short}} in un cluster, si verificano le seguenti azioni:

* I metadati del cluster vengono raccolti e utilizzati per completare l'installazione degli indirizzi IP del nodo di lavoro, delle sottoreti, dell'indirizzo IP e dell'URL Ingress, del CRN del cluster, dell'endpoint di Analisi log, dello spazio e del token.
* Nel tuo account {{site.data.keyword.Bluemix_notm}} vengono generati un ID servizio e una chiave API IAM (Identity and Access Management) in modo che il tuo cluster possa essere identificato da {{site.data.keyword.security-advisor_short}}.
* I componenti {{site.data.keyword.security-advisor_short}} vengono distribuiti nello spazio dei nomi di **monitoraggio** nel tuo cluster.

Se hai più di un cluster, assicurati di eseguire l'installazione per ogni cluster per generare gli ID servizio e le chiavi API per ciascun cluster.


Vuoi saperne di più su come funziona l'analisi di rete in {{site.data.keyword.security-advisor_short}}? [Controlla la documentazione](network-analytics.html).
{: tip}

</br>

## Prerequisiti
{: #cluster_prereqs}

Prima di iniziare, assicurati di soddisfare i seguenti prerequisiti.
{: shortdesc}

Il proprietario dell'account deve completare i passi di installazione. Se non sei tu, verifica chi sia il proprietario del tuo account {{site.data.keyword.Bluemix_notm}} e chiedi assistenza per l'installazione.
{: tip}

Quindi, assicurati di avere i seguenti prerequisiti:

* Una workstation per sviluppatori Mac, Linux o [Windows 10](https://win10faq.com/install-run-ubuntu-bash-windows-10/).
* [Node.js](https://nodejs.org/en/) 6 o superiore
* [jQ](https://stedolan.github.io/jq/download/)
* Le CLI e i plugin necessari nella versione minima richiesta:
  * [CLI {{site.data.keyword.Bluemix_notm}} ](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started) v0.6.5 o superiore
  * [CLI Kubernetes (kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.7 o superiore</br> Il programma di installazione è stato testato con la versione stabile predefinita di {{site.data.keyword.containerlong_notm}} `v1.10.11`. Il programma di installazione non è stato testato con `v1.11.5` o con la versione più recente `v1.12.3`.
  * [Plugin {{site.data.keyword.containerlong_notm}}](https://console.bluemix.net/docs/containers/cs_cli_install.html#cs_cli_install)
  * [Kubernetes Helm (gestore del pacchetto)](https://docs.helm.sh/using_helm/#from-script)
* [Un cluster Kubernetes gratuito o standard](https://console.bluemix.net/containers-kubernetes/catalog/cluster) nella regione {{site.data.keyword.Bluemix_notm}} **Stati Uniti Sud**

Hai bisogno di aiuto per creare e configurare un cluster? Prova a eseguire questa [esercitazione](/docs/containers/cs_tutorials.html#cs_cluster_tutorial).
{: tip}

</br>

## Configurazione di Helm
{: #login}

1.  Accedi a {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud login -a https://api.ng.bluemix.net --sso
  ```
  {: pre}

2.  Elenca tutti i cluster nell'account per ottenere il nome del cluster con cui vuoi lavorare.

  ```
  ibmcloud ks clusters
  ```
  {: pre}

3.  Indirizza la tua CLI al cluster.

  1. Immetti il seguente comando per configurare il cluster. Utilizzerai l'output nel passo successivo.

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

  2. Utilizza l'output del comando precedente per impostare il percorso del file di configurazione Kubernetes locale come variabile di ambiente.

  Esempio:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: screen}

4.  Installa tiller sul tuo cluster in modo da poter lavorare con i grafici Helm.

  ```
  helm init
  ```
  {: pre}

Assicurati di mantenere aperta questa finestra della riga di comando mentre continui.
{: tip}

</br>

## Installazione dei componenti {{site.data.keyword.security-advisor_short}}
{: #cluster_components}

Una volta che il tuo cluster è configurato per lavorare con Helm, puoi installare i componenti {{site.data.keyword.security-advisor_short}}.
{: shortdesc}


Per installare i componenti, continua nella stessa finestra della CLI e completa i seguenti passi:

1. Scarica ed estrai il [pacchetto di installazione](https://github.com/IBM-Bluemix-Docs/security-advisor/blob/master/installation.tar.gz?raw=true).
2. Continuando nella stessa finestra della CLI della sezione precedente, vai all'ubicazione dell'archivio estratto e installa il pacchetto.

  ```
  ./install.sh
  ```
  {: pre}

3.  Verifica che i componenti siano installati correttamente controllando il [dashboard {{site.data.keyword.security-advisor_short}}](https://console.bluemix.net/security-advisor/#/dashboard).

</br>

## Rimozione dei componenti di {{site.data.keyword.security-advisor_short}}
{: #cluster_uninstall}

Rimuovi i componenti di {{site.data.keyword.security-advisor_short}} dal tuo cluster e l'ID servizio e la chiave API dal tuo account {{site.data.keyword.Bluemix_notm}}.

1. Accedi a {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud login -a https://api.ng.bluemix.net --sso
  ```
  {: pre}

2. Elenca tutti i cluster nell'account per ottenere il nome del cluster.

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
