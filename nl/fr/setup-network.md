---

copyright:
  years: 2017, 2019
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

# Network Insights
{: #setup-network}

Avec {{site.data.keyword.security-advisor_long}}, vous pouvez surveiller le comportement en vous servant de l'apprentissage automatique, de modèles appris et du renseignement sur les menaces afin de détecter les conteneurs potentiellement compromis qui s'exécutent dans vos clusters {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Depuis le 20 janvier 2019, Network Insights (bêta) remplace la fonction Network Analytics. Les cartes d'analyse dans votre tableau de bord du service sont supprimées, mais les résultats sont conservés dans la base de données des résultats.
{: important}

## Avant de commencer
{: #network-prereq}

Si vous utilisez la fonction Network Analytics, vous devez [supprimer les composants de service](/docs/services/security-advisor?topic=security-advisor-setup-network#uninstall-analytics) pour pouvoir installer Network Insights. Pour commencer à utiliser Network Insights, assurez-vous de disposer des prérequis suivants :

- Si vous utilisez Windows 10, activez Windows Subsystem for Linux et installez un [shell Ubuntu](https://win10faq.com/install-run-ubuntu-bash-windows-10/){: external}.
- Installez l'interface de ligne de commande yq :
  * Pour [macOS et Windows 10](http://mikefarah.github.io/yq/){: external}.
  * Pour CentOS, Red Hat et Ubuntu, exécutez les commandes suivantes afin d'installer la version 1.15 :
    ```
    wget https://github.com/mikefarah/yq/releases/download/1.15.0/yq_linux_amd64       
    mv yq_linux_amd64 yq   
    chmod +x yq    
    mv yq /usr/local/bin/     
    yq -V
    ```
    {: codeblock}     
- Le fichier binaire cURL mis à jour : pour CentOS et Red Hat, vous pouvez effectuer la mise à jour en exécutant `yum update -y nss curl libcurl`.
- L'interface de ligne de commande [{{site.data.keyword.cloud_notm}} et les plug-in requis](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli).
- L'[interface de ligne de commande Kubernetes](https://kubernetes.io/docs/tasks/tools/install-kubectl/){: external} version 1.10.11 ou ultérieure.
- Le [gestionnaire de package Kubernetes Helm](/docs/containers?topic=containers-helm) version 2.9.0 ou ultérieure.
- Un cluster Kubernetes standard version 1.10.11 ou ultérieure.

## Création d'un compartiment COS
{: #network-setup-cos}

Depuis l'interface graphique de {{site.data.keyword.security-advisor_short}}, vous pouvez créer une instance COS et un compartiment.

1. Dans l'onglet **Intégrations** du tableau de bord du service, définissez **Analyse activée** à la place d'**Analyse désactivée** dans la zone Network Insights.

2. Cliquez sur **Accéder à la configuration**.

3. Dans la section des prérequis, cliquez sur **Créer une instance COS et un compartiment**. Votre instance COS et votre compartiment sont créés automatiquement conformément à la convention de dénomination et avec les droits d'accès IAM appropriés.

Si une instance de COS et un compartiment existent, assurez-vous qu'ils utilisent la convention de dénomination `sa.<account_id>.telemetric.<cos_region>`. Pour que le service puisse lire les données qui sont stockées dans votre instance COS, configurez l'[autorisation service à service](/docs/iam?topic=iam-serviceauth) en utilisant {{site.data.keyword.cloud_notm}} IAM. Associez la zone `source` à `{{site.data.keyword.security-advisor_short}}` et la zone `target` à votre instance COS. Affectez le rôle IAM `Reader`.


## Installation des composants de {{site.data.keyword.security-advisor_short}}
{: #network-install-components}

Vous pouvez installer un agent afin de collecter les journaux de flot réseau depuis votre cluster Kubernetes. Les journaux sont stockés dans votre instance Cloud Object Storage. Vous pouvez ensuite activer Network Insights pour les analyser et détecter et signaler toute activité réseau suspecte.
{: shortdesc}

Assurez-vous de procéder à l'installation pour chaque cluster à surveiller.
{: note}

1. Connectez-vous à l'interface de ligne de commande {{site.data.keyword.cloud_notm}}. Suivez les invites dans l'interface de ligne de commande pour finaliser la connexion.

  ```
  ibmcloud login -a cloud.ibm.com -r <region>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>Région</th>
      <th>Noeud final</th>
    </tr>
    <tr>
      <td>Royaume-Uni</td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>Sud des Etats-Unis</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

2. Définissez le contexte pour votre cluster.

  1. Obtenez la commande permettant de définir la variable d'environnement et téléchargez les fichiers de configuration Kubernetes.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. Copiez la sortie qui commence par `export` et collez-la dans votre terminal pour définir la variable d'environnement `KUBECONFIG`.

3. Obtenez la version de votre cluster Kubernetes.

  ```
  kube_version=$(kubectl version --output json) echo $(echo $kube_version | yq r - serverVersion.major).$(echo $kube_version | yq r - serverVersion.minor)
  ```
  {: codeblock}

3. Si vous utilisez Kubernetes Service v1.12.x, installez Helm à l'aide des commandes suivantes. Dans le cas contraire, consultez la documentation de Kubernetes pour savoir quelles étapes d'installation vous devez suivre pour [configurer Helm dans un cluster avec un accès public](/docs/containers?topic=containers-helm#public_helm_install).

  1. Supprimez les déploiements existants.

    ```
    kubectl delete deployment tiller-deploy -n kube-system
    ```
    {: codeblock}

  2. Appliquez les politiques Tiller RBAC à votre déploiement.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: codeblock}
  
  3. Initialisez Helm dans votre cluster et installez Tiller dans votre cluster.

    ```
    helm init --service-account tiller
    ```
    {: codeblock}

  4. Vérifiez si votre installation a réussi en vérifiant que le statut du pod `tiller-deploy` est bien `running`.

    ```
    kubectl get pods -n kube-system -l app=helm
    ```
    {: codeblock}

4. Vérifiez que votre service Kubernetes appartient à la version 1.11 ou à une version ultérieure puis clonez le référentiel suivant dans votre système local. 

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-network-insights.git
  ```
  {: codeblock}

  La version 1.10 de Kubernetes Service est obsolète et n'est plus prise en charge. Si vous avez déjà installé la version 1.10+, corrigez les vulnérabilités dans l'image existante en redémarrant les pods d'analyseur à l'aide de la commande Helm suivante : `helm upgrade --recreate-pods network-insights`.
  {: deprecated}

5. Placez-vous dans le dossier `security-advisor-network-insights`.

6. Accédez au répertoire `v1.10+`.

7. Procédez à l'extraction du fichier `.tar` en exécutant la commande suivante :

  ```
  tar -xvf security-advisor-network-insights.tar
  ```
  {: codeblock}

8. Placez-vous dans le dossier `security-advisor-network-insights`.

9. Installez Helm en vous référant à la [documentation relative à l'intégration de Kubernetes Service](/docs/containers?topic=containers-helm).

10. Facultatif : [activez le protocole TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md){: external}. Si vous vous servez de votre poste de travail pour gérer l'installation des composants d'analyse dans plusieurs clusters et que le protocole TLS est activé, assurez-vous que les configurations TLS sont les configurations en cours et qu'elles correspondent au cluster en cours dans lequel vous prévoyez d'installer les composants.

11. Exécutez la commande ci-dessous pour installer la charte Helm et ses dépendances. Elle vérifie que votre compartiment utilise la convention de dénomination correcte, crée des secrets Kubernetes, met à jour les valeurs avec l'identificateur global unique de votre cluster, et déploie la charte Helm de Network Insights. Si vous rencontrez une erreur, essayez d'exécuter `helm init --upgrade`.

  ```
  ./network-insight-install.sh <cos_region> <cos_api_key>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>Variable</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><code>cos_region</code></td>
      <td>Région dans laquelle votre instance COS est déployée. Les options sont <code>us-south</code> et <code>eu-gb</code>.</td>
    </tr>
    <tr>
      <td><code>cos_api_key</code></td>
      <td>[Clé d'API](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) que vous avez créée pour accéder à votre instance COS et à votre compartiment. Elle doit posséder le rôle de plateforme `writer`.</td>
    </tr>
  </table>

Vous avez configuré votre cluster en vue de son utilisation avec {{site.data.keyword.security-advisor_short}} Network Insights.



## Suppression des composants
{: #network-delete}

Si vous n'avez plus besoin d'utiliser Network Insights, vous pouvez supprimer les composants de service de votre cluster.
{: shortdesc}

1. Connectez-vous à l'interface de ligne de commande {{site.data.keyword.cloud_notm}}. Suivez les invites dans l'interface de ligne de commande pour finaliser la connexion.

  ```
  ibmcloud login -a cloud.ibm.com -r <region>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>Région</th>
      <th>Noeud final</th>
    </tr>
    <tr>
      <td>Royaume-Uni</td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>Sud des Etats-Unis</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

2. Définissez le contexte pour votre cluster.

  1. Obtenez la commande permettant de définir la variable d'environnement et téléchargez les fichiers de configuration Kubernetes.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. Copiez la sortie qui commence par `export` et collez-la dans votre terminal pour définir la variable d'environnement `KUBECONFIG`.

3. Supprimez les composants à l'aide d'Helm.

  ```
  helm del --purge network-insights [--tls]
  ```
  {: codeblock}

4. Supprimez les secrets Kubernetes.

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}

Assurez-vous de supprimer le processus pour chaque cluster duquel vous voulez retirer les agents.
{: tip}

## Désinstallation de Network Analytics
{: #uninstall-analytics}

Si vous avez utilisé la version bêta de Network Analytics, vous devez désinstaller les anciens composants de {{site.data.keyword.security-advisor_short}} pour pouvoir installer les nouveaux. Veillez à répéter ce processus pour chaque cluster contenant des composants de service.
{: shortdesc}

1. Connectez-vous à {{site.data.keyword.cloud_notm}}.

  ```
  ibmcloud login -a https://api.us-south.ibm.cloud.com --sso
  ```
  {: codeblock}

2. Répertoriez tous les clusters de votre compte pour obtenir le nom du cluster.

  ```
  ibmcloud ks clusters
  ```
  {: codeblock}

3. Définissez le contexte pour votre cluster.

  1. Obtenez la commande permettant de définir la variable d'environnement et téléchargez les fichiers de configuration Kubernetes.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. Copiez la sortie qui commence par `export` et collez-la dans votre terminal pour définir la variable d'environnement `KUBECONFIG`.

4. Accédez à l'emplacement de l'archive extraite et exécutez le script du programme de désinstallation.

  ```
  ./uninstall.sh
  ```
  {: codeblock}

5. Facultatif : désinstallez le composant serveur Helm du cluster.

  ```
  helm reset
  ```
  {: codeblock}
