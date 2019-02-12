---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Tirer parti des services pré-intégrés
{: #setup}

{{site.data.keyword.security-advisor_short}} est fourni avec plusieurs cartes préremplies qui vous aideront à surveiller les risques et menaces de sécurité.
{: shortdesc}

Les services {{site.data.keyword.security-advisor_short}} suivants créent automatiquement une carte pour :

* {{site.data.keyword.registrylong_notm}}
* {{site.data.keyword.cloudcerts_long_notm}}

Même si vous n'avez pas à intervenir pour créer la connexion entre {{site.data.keyword.security-advisor_short}} et les services, vous devez disposer des instances des services configurés avec les informations.


## Surveillance des vulnérabilités dans des images de conteneur
{: #setup_images}

Avec {{site.data.keyword.registryshort_notm}}, vous avez accès à Vulnerability Advisor, qui analyse en permanence les images de votre instance {{site.data.keyword.registryshort_notm}} afin d'y détecter de potentiels problèmes de sécurité. Si des problèmes sont détectés, vous êtes alerté et pouvez afficher un rapport complet dans votre tableau de bord {{site.data.keyword.security-advisor_short}}.
{:shortdesc}

En savoir plus sur [{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html#index).


**Avant de commencer**

Avant de démarrer avec le registre, assurez-vous que les interfaces de ligne de commande et plug-ins suivants sont installés :
* [L'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://clis.ng.bluemix.net/ui/home.html)
* Le plug-in container-registry

  ```
  ibmcloud plugin install container-registry -r Bluemix
  ```
  {: pre}

</br>
**Création d'un espace de nom**

1. Connectez-vous à votre compte via l'interface de ligne de commande (CLI).

  ```
  ibmcloud login --sso
  ```
  {: pre}

2. Connectez-vous à {{site.data.keyword.registryshort_notm}}.

  ```
  ibmcloud cr login
  ```
  {: pre}

3. Facultatif : Créez un espace de nom. Vous pouvez toujours en utiliser un existant.

  ```
  ibmcloud cr namespace-add
  ```
  {: pre}

3. Marquez une image.

  ```
  docker tag <image>:<tag> registry.ng.bluemix.net/<namespace>/<image>:<tag>
  ```
  {: pre}

5. Envoyez l'image par commande push.

  ```
  docker push registry.ng.bluemix.net/<namespace>/<image>:<tag>
  ```
  {: pre}


Une fois que vous avez envoyé vos images par commande push à votre espace de nom {{site.data.keyword.registryshort_notm}}, les informations sur les vulnérabilités détectées s'affichent sur la carte **Images présentant des vulnérabilités** dans le tableau de bord du service. Vous pouvez également explorer en aval des images spécifiques afin de trouver davantage d'informations, comme la description de toutes les vulnérabilités et tous les problèmes de configuration qui ont été identifiés.

</br>

## Surveillance des certificats
{: #setup_certificates}

Saviez-vous qu'{{site.data.keyword.cloudcerts_long_notm}} peut vous aider à surveiller et gérer vos certificats SSL/TLS ? En intégrant {{site.data.keyword.cloudcerts_short}} et {{site.data.keyword.security-advisor_short}}, vous pouvez recevoir des alertes précoces concernant l'expiration de vos certificats, ce qui peut vous aider à prévenir l'indisponibilité d'un service ou d'une application.
{:shortdesc}

En fonction des données d'expiration du certificat que vous téléchargez dans {{site.data.keyword.cloudcerts_short}}, les résultats s'affichent dans le tableau de bord {{site.data.keyword.security-advisor_short}} 90, 60, 10 ou 1 jour avant expiration du certificat. En plus, vous recevez des notifications quotidiennes relatives aux certificats expirés. Les notifications quotidiennes commencent le jour qui suit l'expiration de votre certificat.

Pour déclencher une mise à jour manuelle, vous pouvez tenter de télécharger un certificat valable une journée dans votre instance {{site.data.keyword.cloudcerts_short}}. Une fois l'importation réussie, vous pouvez voir que l'indicateur clé de risques et les résultats sont visibles dans le tableau de bord {{site.data.keyword.security-advisor_short}}.

En savoir plus sur [{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager/index.html#gettingstarted), lisez la documentation.
{: tip}

**Création d'un certificat**

Pour créer un certificat autosigné qui expire au bout d'une journée, vous pouvez exécuter la commande openssl suivante sur votre terminal :

```
openssl req -x509 -newkey rsa:2048 -keyout key.pem -subj "/CN=myservice.com" -out server.pem -days 1 -nodes
```
{: pre}


**Téléchargement d'un certificat**

1. Dans le catalogue {{site.data.keyword.Bluemix_notm}}, recherchez "{{site.data.keyword.cloudcerts_short}}".
2. Donnez un nom à votre instance de service ou utilisez le nom prédéfini.
3. Cliquez sur **Créer**.
4. Pour importer les certificats de votre organisation dans {{site.data.keyword.cloudcerts_short}}, cliquez sur **Importer le certificat**.

A présent que vos certificats sont importés, les informations telles que les délais d'expiration et les certificats arrivés à expiration s'affichent dans la carte **Certificats** du tableau de bord {{site.data.keyword.security-advisor_short}}. En cliquant sur la carte, vous pouvez obtenir des informations plus spécifiques sur les certificats, comme l'instance de service à laquelle appartiennent les certificats. Vous pouvez également voir les étapes que vous pouvez effectuer pour corriger des vulnérabilités en matière de sécurité.

Pour arrêter les notifications, vous devez renouveler votre certificat, le télécharger dans {{site.data.keyword.cloudcerts_short}} et supprimer le certificat expiré.
{: tip}

</br>
</br>
