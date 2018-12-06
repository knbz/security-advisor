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

# Configuration de {{site.data.keyword.security-advisor_short}}
{: #setup}

{{site.data.keyword.security-advisor_long}} vous permet de surveiller en continu vos applications pour les risques et menaces de sécurité. Quand des vulnérabilités sont détectées, vous êtes alerté via le tableau de bord du service.
{:shortdesc}

## Surveillance des vulnérabilités dans des images de conteneur
{: #setup_images}

Une image Docker constitue la base de chaque conteneur que vous créez. Cette image peut contenir votre application, sa configuration, ainsi que les dépendances requises. Une image est généralement stockée dans un registre. En utilisant {{site.data.keyword.registryshort_notm}}, vous disposez d'un accès à Vulnerability Advisor, qui analyse en continu vos images à la recherche de problèmes potentiels de sécurité. Si des problèmes sont détectés, vous êtes alerté et pouvez afficher un rapport complet dans votre tableau de bord {{site.data.keyword.security-advisor_short}}.
{:shortdesc}

En savoir plus sur [{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html#index).


**Avant de commencer**

Avant de démarrer avec le registre, assurez-vous que les interfaces CLI et plug-ins suivants sont installés : 
- [L'interface CLI {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://clis.ng.bluemix.net/ui/home.html)
- Le plug-in container-registry

    ```
    ibmcloud plugin install container-registry -r Bluemix
    ```
    {: pre}

</br>
**Création d'un espace de nom**

1. Connectez-vous à votre compte via l'interface de ligne de commande (CLI). 

   ```
   bx login --sso
   ```
   {: pre}

2. Connectez-vous à {{site.data.keyword.registryshort_notm}}.

   ```
   bx cr login
   ```
   {: pre}

3. Facultatif : Créez un espace de nom. Vous pouvez toujours en utiliser un existant. 

   ```
   bx cr namespace-add
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


Une fois que vous avez envoyé vos images par commande push à votre espace de nom {{site.data.keyword.registryshort_notm}}, les informations sur les vulnérabilités détectées s'affichent sur la carte **Images présentant des vulnérabilités** dans le tableau de bord du service. Vous pouvez également explorer en aval des images spécifiques afin de trouver davantage d'informations, comme la description de toutes les vulnérabilités et tous les problèmes de configuration connus qui ont été identifiés.

</br>

## Surveillance des certificats
{: #setup_certificates}

Saviez-vous qu'{{site.data.keyword.cloudcerts_long_notm}} peut vous aider à surveiller et gérer vos certificats SSL/TLS ? En intégrant {{site.data.keyword.cloudcerts_short}} et {{site.data.keyword.security-advisor_short}}, vous pouvez recevoir des alertes et des rappels pour savoir quand vous devriez mettre à jour vos certificats, ainsi que d'autres informations, ce qui peut vous aider à prévenir les vulnérabilités futures.
{:shortdesc}

En savoir plus sur [{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager/index.html#gettingstarted).

1. Dans le catalogue {{site.data.keyword.Bluemix_notm}}, recherchez "{{site.data.keyword.cloudcerts_short}}".
2. Donnez un nom à votre instance de service ou utilisez le nom prédéfini.
3. Cliquez sur **Créer**.
4. Pour importer les certificats de votre organisation dans {{site.data.keyword.cloudcerts_short}}, cliquez sur **Importer le certificat**.

A présent que vos certificats sont importés, les informations telles que les délais d'expiration et les certificats arrivés à expiration s'affichent dans la carte **Certificats** du tableau de bord {{site.data.keyword.security-advisor_short}}. En cliquant sur la carte, vous pouvez obtenir des informations plus spécifiques sur les certificats, comme l'instance de service à laquelle appartiennent les certificats. Vous pouvez également voir les étapes que vous pouvez effectuer pour corriger des vulnérabilités en matière de sécurité. 
