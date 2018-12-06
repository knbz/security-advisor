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

# Configuration de la surveillance des clients et IP serveur suspects pour un cluster Kubernetes
{: #cluster_install}

Pour essayer la fonction Network Analytics, installez les composants {{site.data.keyword.security-advisor_short}} dans un cluster déployé sur {{site.data.keyword.containerlong_notm}}. Vous pourrez alors visualiser les analyses et alertes réseau dans le tableau de bord {{site.data.keyword.security-advisor_short}}.
{:shortdesc}

Cette fonction de {{site.data.keyword.security-advisor_short}} est un aperçu technique.
{: tip}

Vous souhaitez en apprendre davantage sur Network Analytics dans {{site.data.keyword.security-advisor_short}} ? [Consultez la documentation](network-analytics.html).


## Prérequis
{: #cluster_prereqs}

Avant de commencer :

* Poste de travail développeur Mac, Linux, ou Windows 10
  * Windows 10 : [activez la fonction de sous-système Linux](https://win10faq.com/install-run-ubuntu-bash-windows-10/)
* [Node.js](https://nodejs.org/en/) 6 ou supérieur
* [jQ](https://stedolan.github.io/jq/download/)
* [Interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started) version 0.6.5 ou ultérieure
* [Plug-in interface CLI {{site.data.keyword.containerlong_notm}}](https://console.bluemix.net/docs/containers/cs_cli_install.html#cs_cli_install)
* [Interface CLI Kubernetes (kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) version 1.7 ou ultérieure
* [Kubernetes Helm (gestionnaire de package)](https://docs.helm.sh/using_helm/#from-script)
* [Cluster Kubernetes gratuit ou standard](https://console.bluemix.net/containers-kubernetes/catalog/cluster) dans la région **Sud des Etats-Unis** de {{site.data.keyword.Bluemix_notm}}
* Identifiez le propriétaire de compte {{site.data.keyword.Bluemix_notm}} pour effectuer la procédure d'installation.

## Connexion à votre cluster
{: #login}

1.  Connectez-vous à {{site.data.keyword.Bluemix_notm}}.

    ```
    ibmcloud login -a https://api.ng.bluemix.net --sso
    ```
    {: pre}

2.  Répertoriez tous les clusters du compte pour obtenir le nom du cluster.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

3.  Ciblez votre interface CLI sur le cluster.

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

4.  Définissez le chemin du fichier de configuration Kubernetes local comme variable d'environnement. Exemple :

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: pre}

5.  Configurez Helm dans le cluster.

    ```
    helm init
    ```
    {: pre}

Gardez ouverte cette fenêtre de ligne de commande et continuez.
{: tip}

## Installation des composants de Security Advisor
{: #cluster_components}

Le programme d'installation doit être exécuté par le propriétaire du compte {{site.data.keyword.Bluemix_notm}}.
{: tip}

Lorsque vous configurez {{site.data.keyword.security-advisor_short}}, les actions suivantes ont lieu :
1. Les métadonnées de cluster sont collectées et utilisées pour effectuer l'installation des adresses IP de noeud worker, sous-réseaux, URL et adresse IP Ingress, CRN cluster, noeud final Log Analysis, espace et jeton. 
2. Une clé d'API et un ID service IAM sont générés dans votre compte {{site.data.keyword.Bluemix_notm}} afin que votre cluster puisse être identifié par Security Advisor. Si vous possédez plusieurs clusters, exécutez l'installation sur chacun d'eux afin de générer des ID service et des clés d'API pour chaque cluster. 
3. Les composants {{site.data.keyword.security-advisor_short}} sont déployés dans l'espace de nom **monitoring** de votre cluster.

<br/>
Pour installer les composants de {{site.data.keyword.security-advisor_short}} :

1.  Téléchargez et extrayez le [package d'installation](https://github.com/IBM-Bluemix-Docs/security-advisor/blob/master/installation.tar.gz?raw=true).
2.  Depuis la fenêtre de ligne de commande utilisée à la section précédente, accédez à l'emplacement où l'archive a été extraite et installez le package.

    ```
    ./install.sh
    ```
    {: pre}

3.  Vérifiez que les composants sont installés correctement en consultant le [tableau de bord {{site.data.keyword.security-advisor_short}}](https://console.bluemix.net/security-advisor/#/dashboard).

## Retrait des composants {{site.data.keyword.security-advisor_short}} 
{: #cluster_uninstall}

Retirez les composants {{site.data.keyword.security-advisor_short}} de votre cluster et les ID service et clé d'API de votre compte {{site.data.keyword.Bluemix_notm}}. 

1.  Connectez-vous à {{site.data.keyword.Bluemix_notm}}.

    ```
    ibmcloud login -a https://api.ng.bluemix.net --sso
    ```
    {: pre}

2.  Répertoriez tous les clusters du compte pour obtenir le nom du cluster.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

3.  Ciblez votre interface CLI sur le cluster.

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

4.  Définissez le chemin du fichier de configuration Kubernetes local comme variable d'environnement. Exemple :

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: pre}

5.  Accédez à l'emplacement de l'archive extraite et exécutez le script du programme de désinstallation. 

    ```
    ./uninstall.sh
    ```
    {: pre}

6.  Facultatif : Désinstallez le composant serveur Helm du cluster.

    ```
    helm reset
    ```
    {: pre}
