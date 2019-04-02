---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-13"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

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


# Partenaires 
{: #setup-partner}

Les intégrations d'IBM Business Partner permettent d'afficher des résultats et des alertes critiques de fournisseurs tiers dans le tableau de bord d'{{site.data.keyword.security-advisor_long}}. Ces partenaires ont collaboré avec IBM pour structurer et simplifier l'expérience d'intégration, et vous guider tout au long du processus.
{: shortdesc}

## Avant de commencer
{: #partner-before}

Avant de commencer à intégrer des partenaires, assurez-vous de disposer des prérequis suivants : 

* Assurez-vous de disposer d'un compte avec le partenaire que vous voulez intégrer.
* Assurez-vous de disposer des droits d'administration requis pour générer l'URL d'intégration pour le service partenaire.
* Assurez-vous de disposer d'un accès administrateur IAM à {{site.data.keyword.cloud_notm}} et d'un accès gestionnaire à {{site.data.keyword.security-advisor_short}}.

## Assistant d'intégration
{: #wizard}

En tant qu'administrateur disposant de droits d'administration sur les comptes {{site.data.keyword.cloud_notm}} et partenaire, vous pouvez utiliser l'assistant d'intégration pour être opérationnel rapidement. L'assistant présente quatre étapes :

* Etablissement de la confiance et association de vos comptes {{site.data.keyword.cloud_notm}} et partenaire 
* Copie des informations nécessaires telles que des données d'identification et des URL entre les comptes
* Installation des métadonnées de résultat du partenaire dans {{site.data.keyword.security-advisor_short}}
* Validation de l'appariement via la publication d'un résultat du partenaire dans {{site.data.keyword.security-advisor_short}}


## Intégration de NeuVector
{: #setup-neuvector}

Avec NeuVector, vous pouvez détecter les menaces et violations de réseau pour éviter toute attaque de vos applications basées sur un conteneur. Par le biais de la surveillance, vous pouvez éviter toute exploitation et tout accès à des informations sensibles ou toute obtention de privilèges supplémentaires en détectant les escalades des privilèges de superutilisateur, les analyses de port, les shells inversés et les activités suspectes au niveau du système de fichiers dans vos conteneurs et sur vos hôtes.
{: shortdesc}

Pour intégrer NeuVector à {{site.data.keyword.security-advisor_short}}, vous pouvez procéder comme suit : 

1. Accédez à **Intégrations > Intégrations de partenaire** et sélectionnez **NeuVector** parmi les options proposées. 
2. Connectez vos comptes.
  1. Entrez un nom pour votre connexion.
  2. Indiquez l'URL du tableau de bord NeuVector.
  3. Indiquez l'URL de configuration de NeuVector.
    1. Connectez-vous à NeuVector et accédez à la section des paramètres.
    2. Cliquez sur **Generate URL**.
    3. Copiez l'URL et collez-la dans l'assistant d'intégration de {{site.data.keyword.security-advisor_short}}.
  4. Cliquez sur **Next**.
3. Vérifiez que vous avez autorisé le service à générer une clé d'API et un ID de service et créez la connexion au service en cliquant sur **Next**.
4. Cliquez sur **Done**.
5. Accédez au tableau de bord de votre service pour examiner le test de résultats envoyé à {{site.data.keyword.containershort_notm}} par NeuVector.



## Intégration de Twistlock
{: #setup-twistlock}

Vous pouvez éviter tout risque en arrêtant le déploiement d'images vulnérables dans votre environnement. Avec l'application de règles automatisées et personnalisées, Twistlock permet un contrôle complet à chaque étape du cycle de vie de l'application.
{: shortdesc}

Lorsque vous configurez la connexion partenaire, deux cartes qui récapitulent les résultats de Twistlock sont créées dans votre tableau de bord. 

**Twistlock Runtime** introduit deux nouveaux indicateurs clés de risques : 

* Total incidents and audits (Nombre total d'incidents et d'audits) : résultats liés aux incidents ou aux audits dans vos charges de travail natives du cloud. 
* Total firewall audits (Nombre total d'audits de pare-feu) : résultats liés aux problèmes relatifs à votre pare-feu.

**Twistlock vulnerabilities** introduit un indicateur clé de risques : 

* Total images with new vulnerabilities (Nombre total d'images comportant de nouvelles vulnérabilités) : résultats liés aux vulnérabilités détectées dans vos images de conteneur. 

Consultez la documentation de Twistlock pour en savoir plus sur la société. 

### Configuration de Twistlock
{: #configure-twistlock}

1. Dans le tableau de bord {{site.data.keyword.security-advisor_short}}, accédez à **Intégrations > Intégrations de partenaire** et sélectionnez **Twistlock** parmi les options proposées. 

2. Cliquez sur **Oui, connectez mon compte à {{site.data.keyword.security-advisor_short}}**.

  Si vous ne possédez pas encore de compte, cliquez sur **Non, aidez-moi à créer un compte > Créer un compte**. Vous pouvez remplir le formulaire ; l'équipe commerciale de Twistlock vous contactera pour la mise en route.
  {: note}

3. Attribuez un nom à votre connexion. Assurez-vous que le nom est unique pour votre compte et veillez à n'utiliser que des caractères alphanumériques, des espaces et des traits d'union. 

4. Facultatif : si vous ne possédez pas encore d'URL de configuration, cliquez sur **Générer une URL d'enregistrement** pour accéder à la documentation de Twistlock et apprendre à créer l'URL. Assurez-vous de disposer du jeton Twistlock fourni avec votre licence pour l'accès à la documentation. 

5. Dans le tableau de bord Twistlock, accédez à **Manage > Alerts** et cliquez sur **Add profile**.

6. Sélectionnez **{{site.data.keyword.security-advisor_short}}** dans la liste déroulante **Provider**. 

7. Cliquez sur **Copy** pour utiliser l'URL de configuration fournie. 

8. Dans le tableau de bord {{site.data.keyword.security-advisor_short}}, collez l'URL dans la zone **Entrer l'URL de configuration Twistlock**. 

9. Cliquez sur **Connecter un partenaire**.

### Vérification de la connexion 
{: #twistlock-verify}

1. Dans le tableau de bord {{site.data.keyword.security-advisor_short}}, vérifiez que les cartes Twistlock s'affichent comme prévu. 

2. Dans le tableau de bord Twistlock, actualisez l'onglet **Alerts**. La connexion {{site.data.keyword.security-advisor_short}} doit s'afficher. Ouvrez-la. 

3. Vérifiez que les types d'alerte pour lesquels vous voulez être notifié sont sélectionnés, puis cliquez sur **Verify** pour vous assurer que la connexion est établie. 
