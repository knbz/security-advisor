---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

keywords: Centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

---

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


# Connexions directes
{: #setup-custom-gui}

Avec {{site.data.keyword.security-advisor_short}}, vous pouvez intégrer vos outils de sécurité personnalisés existants, qu'il s'agisse de services open source, de services tiers ou de services développés sur mesure. Vous pouvez créer une connexion directe de {{site.data.keyword.security-advisor_short}} à l'autre outil en ajoutant l'URL à votre liste d'intégrations. En créant le lien, vous pouvez facilement surveiller tous les outils que vous utilisez.
{: shortdesc}


## Avant de commencer
{: #custom-before-gui}

Pour pouvoir ajouter l'intégration, vous devez disposer d'un compte avec le partenaire que vous voulez intégrer.

{{site.data.keyword.security-advisor_short}} ne conserve pas les données d'identification associées au service partenaire. Les utilisateurs de l'entreprise doivent s'authentifier à l'aide de SAML auprès d'{{site.data.keyword.cloud_notm}} et du partenaire commercial.
{: note}

## Configuration de la connexion
{: #custom-configure-connection}

1. Connectez-vous à votre outil de sécurité et obtenez votre URL unique.

2. Connectez-vous à {{site.data.keyword.cloud_notm}} depuis la console.

3. Cliquez sur **Intégrations personnalisées > Direct Connection**. Un écran s'affiche.

  1. Attribuez un nom à votre solution. Le nom ne peut comporter que des caractères alphanumériques, des espaces et des traits d'union.

  2. Entrez l'URL de la solution au format : `www.<website>.<domain>`.

  3. Téléchargez une icône ou une image pour représenter l'outil.

  4. Cliquez sur **Connecter** pour finaliser la configuration. {{site.data.keyword.security-advisor_short}} crée les artefacts requis pour l'intégration, tels que l'ID de service, la clé d'API, l'ID de compte et les métadonnées. Le rôle `writer` est affecté.
