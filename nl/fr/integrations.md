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

# Intégrations
{: #about}

Avec {{site.data.keyword.security-advisor_long}}, vous pouvez intégrer des résultats issus de vos services IBM Cloud et solutions partenaires ou créer vos propres intégrations personnalisées.
{: shortdesc}


## Résultats pré-intégrés
{: #built-in}

{{site.data.keyword.security-advisor_long}} vous permet d'approfondir vos connaissances en matière de problèmes potentiels par le biais des fonctions de services intégrés.
{: shortdesc}


{{site.data.keyword.security-advisor_short}} prêt à l'emploi s'intègre à :

* Certificate Manager pour les notifications liées au cycle de vie et à l'expiration des certificats.
* Vulnerability Advisor pour les détails relatifs à la vulnérabilité des images et aux problèmes de configuration.

Pour en savoir plus, voir la rubrique [Tirer parti des services pré-intégrés](setup.html)!

</br>

## Solutions partenaires
{: #partner}

Les solutions partenaires permettent de renforcer la sécurité de vos déploiements IBM Cloud en créant un lien entre {{site.data.keyword.security-advisor_short}} et d'autres outils de sécurité situés en dehors d'IBM Cloud.
{: shortdesc}

{{site.data.keyword.security-advisor_short}} est actuellement en partenariat avec les sociétés suivantes :

### NeuVector
{: #neuvector}

[NeuVector](https://neuvector.com/) qui offre une plateforme de sécurité automatisée hautement intégrée pour Kubernetes et Red Hat OpenShift permettant de surveiller et de protéger le trafic réseau de conteneurs. Plus particulièrement le trafic réseau Est-Ouest.

Avec NeuVector, vous pouvez détecter les menaces et violations de réseau de manière à éviter les attaques au niveau de vos applications basées sur conteneur. Par le biais de la surveillance, vous pouvez vous prémunir des utilisations et coupures en détectant les pics de privilèges, les scans de ports, les interpréteurs de commandes inversés et les activités suspectes au niveau du système de fichiers dans vos conteneurs et hôtes.

Pour savoir comment configurer votre instance NeuVector avec l'assistant d'intégration, voir [Solutions partenaires](partners.html).

Vous êtes un partenaire et intéressé par l'intégration de votre solution à {{site.data.keyword.security-advisor_short}} ? Prenez contact avec notre équipe par le biais de Matt Ward à l'adresse wardm@us.ibm.com.
{: tip}

</br>

## Intégrations personnalisées
{: #custom}

Vous disposez sans doute déjà d'un outil de sécurité sur lequel vous pouvez compter. Vous pouvez intégrer cet outil au tableau de bord {{site.data.keyword.security-advisor_short}} pour que toutes vos informations de sécurité soient centralisées en un même point.
{: shortdesc}

**Intégration de vos propres outils à l'aide de l'interface graphique**

A l'aide de l'interface graphique de {{site.data.keyword.security-advisor_short}}, vous pouvez ajouter un signet à vos outils de sécurité internes et externes de manière à disposer d'un point d'accès à tous les éléments depuis le tableau de bord {{site.data.keyword.security-advisor_short}}. Par exemple, vous pouvez ajouter un signet à PagerDuty.

**Intégration de vos propres outils à l'aide de l'API**

A l'aide de l'API Findings, vous pouvez intégrer des résultats depuis vos outils de sécurité personnalisés dans le tableau de bord {{site.data.keyword.security-advisor_short}}. Il peut s'agir de n'importe outil personnalisé ou partenaire générant une alerte de sécurité ou un résultat qui prend également en charge l'interaction basée sur une API.

**Commencer**

Pour connaître les pratiques recommandées en matière d'optimisation des API et de création de signets à l'aide de l'interface graphique, voir [Outils de sécurité personnalisés](/docs/services/security-advisor/custom.html).

</br>


## Analyse réseau (prévisualisation)
{: #network-analytics}

La prévisualisation d'analyse réseau vous permet de détecter les menaces en surveillant vos communications de cluster {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Surveiller les communications de cluster en vue de détecter les communications suspectes vers et en provenance de vos clusters vous permet de tirer parti des renseignements sur les menaces fournis par IBM X-Force pour signaler tout problème potentiel. Lorsqu'une communication suspecte est identifiée, un résultat que vous pouvez analyser et évaluer est renvoyé.

Pour des informations plus détaillées sur l'analyse réseau, voir [Analyse réseau](network-analytics.html).
{: tip}

</br>
</br>
