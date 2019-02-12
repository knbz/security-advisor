---

copyright:
  years: 2018
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

# Solutions partenaires
{: #setup-partner}

Les solutions partenaires permettent de renforcer la sécurité en créant un lien entre {{site.data.keyword.security-advisor_long}} et d'autres outils de sécurité.
{: shortdesc}

## Assistant d'intégration
{: #wizard}

En tant qu'administrateur doté de droits d'administration sur les comptes IBM Cloud et partenaire, vous pouvez utiliser l'assistant d'intégration pour être rapidement opérationnel. L'assistant se déroule en quatre étapes :

* Etablir des relations de confiance et associer vos comptes IBM Cloud et partenaire
* Copier les informations nécessaires telles que des données d'identification et des URL entre les comptes
* Installer les métadonnées de résultat du partenaire dans {{site.data.keyword.security-advisor_short}}
* Valider l'appariement en envoyant des résultats du partenaire à {{site.data.keyword.security-advisor_short}}

</br>

## Intégration de NeuVector
{: #neuvector}

**Avant de commencer**

* Vérifiez que vous disposez d'un compte avec le partenaire que vous voulez intégrer.
* Vérifiez que vous disposez des droits d'administration requis pour générer l'URL d'intégration pour le service partenaire.
* Vérifiez que vous disposez d'un accès administrateur IAM à IBM Cloud et d'un accès gestionnaire à {{site.data.keyword.security-advisor_short}}.

**Intégration**

1. Connectez vos comptes.
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
