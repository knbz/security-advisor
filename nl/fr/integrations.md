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


# Intégrations
{: #integrations}

Avec {{site.data.keyword.security-advisor_long}}, vous pouvez ajouter d'autres analyses intégrées et solutions partenaires ou créer vos propres intégrations personnalisées.
{: shortdesc}


## Résultats préintégrés
{: #integrate-built-in}

Avec {{site.data.keyword.security-advisor_long}}, vous pouvez explorer les problèmes potentiels à l'aide de fonctions de services intégrés.
{: shortdesc}


Clé en main, {{site.data.keyword.security-advisor_short}} s'intègre à :

* Certificate Manager pour les notifications liées au cycle de vie et à l'expiration des certificats.
* Vulnerability Advisor pour les détails relatifs à la vulnérabilité des images et aux problèmes de configuration.

Pour en savoir plus, voir [Tirer parti des services préintégrés](/docs/services/security-advisor?topic=security-advisor-setup-services)!

</br>

## Intégrations de partenaire
{: #integrate-partner}

Les intégrations de partenaire permettent d'améliorer la sécurité pour vos déploiements {{site.data.keyword.cloud_notm}} en créant une connexion entre {{site.data.keyword.security-advisor_short}} et d'autres outils de sécurité qui collaborent avec IBM pour fournir une expérience utilisateur harmonieuse.
{: shortdesc}

Les partenaires actuels de {{site.data.keyword.security-advisor_short}} sont NeuVector et Twistlock.

Vous êtes un partenaire et intéressé par l'intégration de votre solution à {{site.data.keyword.security-advisor_short}} ? Prenez contact avec notre équipe par l'intermédiaire de Matt Ward à l'adresse wardm@us.ibm.com.
{: tip}

### NeuVector
{: #integrate-neuvector}

[NeuVector](https://neuvector.com){: external} offre une plateforme de sécurité automatisée hautement intégrée pour Kubernetes et Red Hat OpenShift permettant de surveiller et de protéger le trafic réseau de conteneurs, et plus particulièrement le trafic réseau Est-Ouest.

Avec NeuVector, vous pouvez détecter les menaces et violations de réseau pour éviter toute attaque de vos applications basées sur un conteneur. Par le biais de la surveillance, vous pouvez éviter toute exploitation et tout accès à des informations sensibles ou toute obtention de privilèges supplémentaires en détectant les escalades des privilèges de superutilisateur, les analyses de port, les shells inversés et les activités suspectes au niveau du système de fichiers dans vos conteneurs et sur vos hôtes.

Pour de l'aide afin de configurer votre instance NeuVector, voir [Solutions partenaires](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-neuvector).


### Twistlock
{: #integrate-twistlock}

[Twistlock](https://www.twistlock.com){: external} est le seul à pouvoir prévenir les risques dans l'ensemble du SDLC en empêchant le déploiement d'images vulnérables dans votre environnement. L'application de règles automatisées et personnalisées permet un contrôle complet à chaque étape du cycle de vie de l'application.? Twistlock Intelligence Stream identifie et agrège les informations relatives à la vulnérabilité directement depuis 30 projets en amont (ou plus), sources commerciales et recherche exclusive des laboratoires Twistlock.

La priorité est de disposer des données les plus précises possible afin de couvrir toutes les couches de votre pile et de bénéficier d'une visibilité parfaite et du taux de faux positifs le plus faible.??Twistlock combine ces données avec les informations relatives aux déploiements réels, par exemple les conteneurs qui sont exposés sur Internet, ceux qui s'exécutent avec des privilèges élevés, et ceux pour lesquels des mesures de sécurité sont en place ; ainsi, vous savez toujours quelles sont les vulnérabilités les plus critiques dans votre environnement particulier.

Pour de l'aide afin de configurer votre instance NeuVector, voir [Solutions partenaires](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-twistlock).
</br>


## Intégrations personnalisées
{: #integrate-custom}

Vous disposez probablement déjà d'un outil de sécurité sur lequel vous pouvez compter. Vous pouvez intégrer cet outil au tableau de bord {{site.data.keyword.security-advisor_short}} pour que toutes vos informations de sécurité soient centralisées en un même point.
{: shortdesc}

### Création d'une connexion directe
{: #create-direct-connect}

A l'aide de l'interface graphique de {{site.data.keyword.security-advisor_short}}, vous pouvez ajouter un signet à vos outils de sécurité internes et externes de manière à disposer d'un point d'accès à tous les éléments depuis le tableau de bord {{site.data.keyword.security-advisor_short}}. Par exemple, vous pouvez ajouter un signet pour PagerDuty.

### Intégration de vos propres outils à l'aide de l'API
{: #integrate-tools-api}

A l'aide de l'API Findings, vous pouvez intégrer des résultats depuis vos outils de sécurité personnalisés dans le tableau de bord {{site.data.keyword.security-advisor_short}}. Il peut s'agir de n'importe quel outil personnalisé ou partenaire générant une alerte de sécurité ou un résultat qui prend également en charge l'interaction basée sur une API.

## Built-in Insights
{: #integrate-insights}

Avec Built-in Insights, vous pouvez détecter les problèmes potentiels en surveillant en permanence votre cluster et vos journaux de compte. En surveillant le trafic réseau et l'activité d'utilisateur, vous pouvez garantir la protection de vos ressources {{site.data.keyword.cloud_notm}}.
{: shortdesc}

### Network Insights (bêta)
{: #integrate-network-insights}

Avec Network Insights (bêta), vous pouvez surveiller et analyser les communications réseau du cluster, entrantes comme sortantes, entre votre cluster Kubernetes et des entités externes. Avec le renseignement sur les menaces et la détection des anomalies, le service peut identifier les attaques de reconnaissance et les actifs potentiellement compromis. Pour en savoir plus, voir [Network Insights](/docs/services/security-advisor?topic=security-advisor-network).

### Activity Insights (aperçu)
{: #integrate-activity-insights}

Avec Activity Insights (aperçu), vous pouvez surveiller en permanence vos journaux {{site.data.keyword.cloud_notm}} Activity Tracker pour identifier toute activité non autorisée ou suspecte de la part d'utilisateurs ou d'applications à l'aide de packages de règles. Vous pouvez utiliser les packages de règles mis à disposition par le service, qui reposent sur les meilleures pratiques en matière de sécurité, ou personnaliser les règles pour les adapter à vos besoins. Pour en savoir plus, voir [Activity Insights](/docs/services/security-advisor?topic=security-advisor-activity).
