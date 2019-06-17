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

Avec {{site.data.keyword.security-advisor_long}}, vous pouvez surveiller en permanence vos journaux {{site.data.keyword.cloud_notm}} Activity Tracker pour identifier les comportements et les changements non autorisés ou suspects dans vos ressources. Vous pouvez utiliser des packages de règles s'appuyant sur les pratiques recommandées fournies par le service ou créer vos propres règles personnalisées.
{: shortdesc}


## Avant de commencer
{: #activity-prereq}

Avant de commencer à utiliser Activity Insights, assurez-vous de disposer des prérequis suivants :

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
{: #activity-setup-cos}

Depuis l'interface graphique de {{site.data.keyword.security-advisor_short}}, vous pouvez créer une instance COS et un compartiment.

1. Accédez à l'onglet **Intégrations** du tableau de bord du service.

2. Définissez **Analyse activée** à la place d'**Analyse désactivée** dans la zone Activity Insights.

3. Cliquez sur **Accéder à la configuration**.

4. Dans la section des prérequis, cliquez sur **Créer une instance COS et un compartiment**. Votre instance COS et votre compartiment sont créés automatiquement conformément à la convention de dénomination et avec les droits d'accès IAM appropriés. Les informations sur le compartiment sont affichées.

Si une instance de COS et un compartiment existent, assurez-vous qu'ils utilisent la convention de dénomination `sa.<account_id>.telemetric.<cos_region>`. Pour que le service puisse lire les données qui sont stockées dans votre instance COS, configurez l'[autorisation service à service](/docs/iam?topic=iam-serviceauth) en utilisant {{site.data.keyword.cloud_notm}} IAM. Associez la zone `source` à `{{site.data.keyword.security-advisor_short}}` et la zone `target` à votre instance COS. Affectez le rôle IAM `Reader`.


## Installation des composants de {{site.data.keyword.security-advisor_short}}
{: #activity-install-components}

Vous pouvez installer un agent afin de collecter les journaux de flux d'audit depuis votre compte {{site.data.keyword.cloud_notm}}. Les journaux sont stockés dans votre instance Cloud Object Storage, dans laquelle vous pouvez activer Activity Insights afin d'analyser les journaux pour identifier tout comportement suspect.
{: shortdesc}

1. Clonez le référentiel suivant dans votre système local :

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. Placez-vous dans le dossier `security-advisor-activity-insights`.

3. Procédez à l'extraction du fichier `.tar` en exécutant la commande suivante :

  ```
  tar -xvf security-advisor-activity-insights.tar
  ```
  {: codeblock}

4. Placez-vous dans le dossier `security-advisor-activity-insights`.
5. Connectez-vous à l'interface de ligne de commande {{site.data.keyword.cloud_notm}}. Suivez les invites dans l'interface de ligne de commande pour finaliser la connexion.

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

6. Définissez le contexte pour votre cluster.

  1. Obtenez la commande permettant de définir la variable d'environnement et téléchargez les fichiers de configuration Kubernetes.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. Copiez la sortie qui commence par `export` et collez-la dans votre terminal pour définir la variable d'environnement `KUBECONFIG`.

7. Installez Helm en vous référant à la [documentation relative à l'intégration de Kubernetes Service](/docs/containers?topic=containers-helm).

8. Facultatif : [activez le protocole TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md){: external}. Si vous vous servez de votre poste de travail pour gérer l'installation des composants d'analyse dans plusieurs clusters et que le protocole TLS est activé, assurez-vous que les configurations TLS sont les configurations en cours et qu'elles correspondent au cluster en cours dans lequel vous prévoyez d'installer les composants.

9. Exécutez la commande ci-dessous pour installer Insights. Elle valide la convention de dénomination de votre compartiment, crée des secrets Kubernetes, met à jour les valeurs avec l'identificateur global unique de votre cluster, et déploie Activity Insights.

  ```
  ./activity-insight-install.sh <cos_region> <cos_api_key> <at_region> <account_api_key> <account_spaces>
  ```
  {: codeblock}

  Si vous rencontrez une erreur, essayez d'exécuter `helm init --upgrade`.
  {: tip}

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
    <tr>
      <td><code>at_region</code></td>
      <td>Région dans laquelle vous avez créé votre instance COS et votre compartiment. Les options sont <code>us-south</code> et <code>eu-gb</code>.</td>
    </tr>
    <tr>
      <td><code>account_api_key</code></td>
      <td>Clé d'API de la plateforme pour votre compte {{site.data.keyword.cloud_notm}}.</td>
    </tr>
    <tr>
      <td><code>account_spaces</code></td>
      <td>Liste des identificateurs globaux uniques d'espace séparés par une virgule pour votre compte {{site.data.keyword.cloud_notm}}.</td>
    </tr>
  </table>


## Ajout de packages de règles à COS
{: #activity-adding-rules}

Un package de règles est un fichier JSON qui contient la liste des règles que vous voulez surveiller. Vous pouvez télécharger des packages de règles ou [créer vos propres packages de règles](/docs/services/security-advisor?topic=security-advisor-activity#activity-packages). Le moteur {{site.data.keyword.security-advisor_short}} vérifie que la syntaxe de chaque règle est correcte.
{: shortdesc}

1. Clonez le référentiel ci-dessous pour obtenir plusieurs packages de règles prédéfinis. Un dossier portant le nom `security-advisor-activity-insights` est créé dans votre système local.

  ```
  https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. Localement, créez un dossier portant le nom `IBM.rules/activities`.

3. Copiez les fichiers JSON de `security-advisor-activity-insights/security-advisor-ata-rule-packages` à `IBM.rules/activities`.

4. Accédez à votre tableau de bord {{site.data.keyword.cloud_notm}} et sélectionnez l'instance de service COS qui est associée à Activity Insights.

5. Dans l'onglet **Compartiments** du tableau de bord du service, sélectionnez le compartiment qui est associé à Activity Insights.

5. Dans la page d'accueil de l'instance COS, cliquez sur **Télécharger** et sélectionnez **Dossiers**.

6. Si vous y êtes invité, cliquez sur **Installer le client Aspera Connect**. Si vous n'y êtes pas invité, cela signifie que le client est déjà installé. Si vous devez installer le client, répétez l'étape 5 une fois l'installation terminée.

7. Sélectionnez le dossier *IBM.rules/activities*.

8. Cliquez sur **Télécharger**.

Vous voulez utiliser vos propres packages ? Servez-vous de l'un des fichiers JSON comme modèle et créez des règles adaptées aux besoins de votre organisation. Une fois le fichier créé, ajoutez-le dans le dossier *IBM.rules/activities* dans votre instance COS. Pour plus d'informations sur les types de règles et le formatage, voir [Présentation des packages de règles](/docs/services/security-advisor?topic=security-advisor-activity).
{: tip}


## Suppression des composants
{: #activity-delete}

Si vous n'avez plus besoin d'utiliser Activity Insights, vous pouvez supprimer les composants de service de votre cluster.
{: shortdesc}

1. Supprimez les composants de service à l'aide d'Helm. Assurez-vous d'utiliser l'indicateur `-tls` si le protocole TLS est activé.

  ```
  helm del --purge activity-insights [--tls]
  ```
  {: codeblock}

2. Supprimez les secrets Kubernetes.

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}
