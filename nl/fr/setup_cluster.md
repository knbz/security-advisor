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

# Configuration de l'analyse réseau (bêta)
{: #cluster_install}

Vous pouvez expérimenter les fonctions d'analyse réseau du service en installant {{site.data.keyword.security-advisor_short}} dans un cluster {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

L'analyse réseau est une fonction de prévisualisation disponible uniquement dans la région du Sud des Etats-Unis.
{: note}

Lorsque vous configurez {{site.data.keyword.security-advisor_short}} dans un cluster, les actions suivantes ont lieu :

* Les métadonnées de cluster sont collectées et utilisées pour effectuer l'installation des adresses IP de noeud worker, sous-réseaux, URL et adresse IP Ingress, CRN cluster, noeud final Log Analysis, espace et jeton.
* Une clé d'API et un ID de service IAM (Identity and Access Management, gestion des identités et des accès) sont générés dans votre compte {{site.data.keyword.Bluemix_notm}} afin que {{site.data.keyword.security-advisor_short}} puisse identifier votre cluster.
* Les composants {{site.data.keyword.security-advisor_short}} sont déployés dans l'espace de nom **monitoring** de votre cluster.

Si vous possédez plusieurs clusters, veillez à exécuter l'installation sur chacun d'eux afin de générer des ID de service et des clés d'API pour chaque cluster. 


Vous souhaitez en apprendre davantage sur le fonctionnement de l'analyse réseau dans {{site.data.keyword.security-advisor_short}} ? [Consultez la documentation](network-analytics.html).
{: tip}

</br>

## Prérequis
{: #cluster_prereqs}

Avant de commencer, assurez-vous que les conditions préalables suivantes sont remplies.
{: shortdesc}

Le propriétaire de compte doit effectuer les étapes d'installation. Si ce n'est pas vous, vérifiez qui est propriétaire de votre compte {{site.data.keyword.Bluemix_notm}} et demandez-lui de vous aider dans cette procédure.
{: tip}

Ensuite, assurez-vous de disposer de la configuration requise suivante :

* Un poste de travail développeur Mac, Linux ou [Windows 10](https://win10faq.com/install-run-ubuntu-bash-windows-10/).
* [Node.js](https://nodejs.org/en/) 6 ou supérieur
* [jQ](https://stedolan.github.io/jq/download/)
* Les interfaces de ligne de commande et plug-ins nécessaires dans la version minimale requise :
  * [Interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started) version 0.6.5 ou ultérieure
  * [Interface de ligne de commande Kubernetes (kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) version 1.7 ou ultérieure</br> Le programme d'installation a été testé avec la version stable par défaut de {{site.data.keyword.containerlong_notm}}, la `version 1.10.11`. Le programme d'installation n'a pas été testé avec la `version 1.11.5` ou la dernière version `1.12.3`.
  * [Plug-in {{site.data.keyword.containerlong_notm}}](https://console.bluemix.net/docs/containers/cs_cli_install.html#cs_cli_install)
  * [Kubernetes Helm (gestionnaire de package)](https://docs.helm.sh/using_helm/#from-script)
* [Un cluster Kubernetes gratuit ou standard](https://console.bluemix.net/containers-kubernetes/catalog/cluster) dans la région **Sud des Etats-Unis** de {{site.data.keyword.Bluemix_notm}}

Besoin d'aide pour créer et configurer un cluster ? Essayez avec ce [tutoriel](/docs/containers/cs_tutorials.html#cs_cluster_tutorial).
{: tip}

</br>

## Configuration de Helm
{: #login}

1.  Connectez-vous à {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud login -a https://api.ng.bluemix.net --sso
  ```
  {: pre}

2.  Répertoriez tous les clusters du compte pour trouver le nom du cluster avec lequel vous voulez travailler.

  ```
  ibmcloud ks clusters
  ```
  {: pre}

3.  Ciblez votre interface de ligne de commande sur le cluster.

  1. Exécutez la commande suivante pour configurer le cluster. Vous utiliserez la sortie de cette commande dans la prochaine étape.

    ```
    ibmcloud ks cluster-config <nom_ou_id_cluster>
    ```
    {: pre}

  2. Utilisez la sortie de la commande précédente pour définir le chemin d'accès au fichier de configuration du Kubernetes local en tant que variable d'environnement.

  Exemple :

    ```
    export KUBECONFIG=/Users/<nom_utilisateur>/.bluemix/plugins/container-service/clusters/<nom_cluster>/kube-config-prod-dal10-<nom_cluster>.yml
    ```
    {: screen}

4.  Installez la barre sur votre cluster pour pouvoir travailler avec des chartes Helm.

  ```
  helm init
  ```
  {: pre}

Veillez à garder ouverte cette fenêtre de ligne de commande pendant que vous continuez.
{: tip}

</br>

## Installation des composants {{site.data.keyword.security-advisor_short}}
{: #cluster_components}

Une fois votre cluster configuré pour fonctionner avec Helm, vous pouvez installer des composants {{site.data.keyword.security-advisor_short}}.
{: shortdesc}


Pour installer les composants, restez dans la même fenêtre d'interface de ligne de commande et procédez comme suit :

1. Téléchargez et extrayez le [package d'installation](https://github.com/IBM-Bluemix-Docs/security-advisor/blob/master/installation.tar.gz?raw=true).
2. Toujours depuis la fenêtre de ligne de commande utilisée à la section précédente, accédez à l'emplacement où l'archive a été extraite et installez le package.

  ```
  ./install.sh
  ```
  {: pre}

3.  Vérifiez dans le [tableau de bord {{site.data.keyword.security-advisor_short}}](https://console.bluemix.net/security-advisor/#/dashboard) que les composants sont correctement installés.

</br>

## Retrait des composants {{site.data.keyword.security-advisor_short}}
{: #cluster_uninstall}

Retirez les composants {{site.data.keyword.security-advisor_short}} de votre cluster et les ID service et clé d'API de votre compte {{site.data.keyword.Bluemix_notm}}.

1. Connectez-vous à {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud login -a https://api.ng.bluemix.net --sso
  ```
  {: pre}

2. Répertoriez tous les clusters du compte pour obtenir le nom du cluster.

  ```
  ibmcloud ks clusters
  ```
  {: pre}

3. Ciblez votre interface de ligne de commande sur le cluster.

  ```
  ibmcloud ks cluster-config <nom_ou_id_cluster>
  ```
  {: pre}

4. Définissez le chemin du fichier de configuration Kubernetes local comme variable d'environnement. Exemple :

  ```
  export KUBECONFIG=/Users/<nom_utilisateur>/.bluemix/plugins/container-service/clusters/<nom_cluster>/kube-config-prod-dal10-<nom_cluster>.yml
  ```
  {: pre}

5. Accédez à l'emplacement de l'archive extraite et exécutez le script du programme de désinstallation.

  ```
  ./uninstall.sh
  ```
  {: pre}

6. Facultatif : Désinstallez le composant serveur Helm du cluster.

  ```
  helm reset
  ```
  {: pre}
