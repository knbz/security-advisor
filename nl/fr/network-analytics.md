---

copyright:
  years: 2018
lastupdated: "2018-11-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}

# Network Analytics
{: #network-analytics}

{{site.data.keyword.security-advisor_long}} d'avoir une connaissance approfondie des communications réseau potentiellement dangereuses liées à vos cluster {{site.data.keyword.containerlong_notm}}. Pou prévisualiser cette fonction d'analyse réseau, sélectionnez **Aperçu** sur la [page de mise en route](https://console.bluemix.net/security/advisor/#!/overview) de {{site.data.keyword.security-advisor_short}}.
{: shortdesc}

La fonction de prévisualisation d'analyse réseau se compose de trois modules : 

1. **Agent de collecte de flux réseau** : Installé sur votre cluster, l'agent collecte des informations réseau et les envoie au système de back end d'analyse. En savoir plus sur la [collecte de données](#data-collection).

2. **Système de back end d'analyse** : Le système identifie les communications suspectes vers et depuis le cluster. Il utilise les renseignements sur les menaces fournis par [IBM X-Force![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/security/xforce) pour interpréter les informations réseau. Quand une communication suspecte est identifiée, le système de back end d'analyse surveille la communication et envoie des résultats (findings) de sécurité au tableau de bord {{site.data.keyword.security-advisor_short}}. 

3. **Tableau de bord {{site.data.keyword.security-advisor_short}}** : Les résultats (findings) et indicateurs clés de performance (KPI) du système de back end d'analyse sont présentés dans deux cartes : 

   - **Clients suspects (trafic entrant)** : Cette carte montre les indicateurs clés de performance et les résultats concernant des clients externes suspects qui accèdent au cluster, par exemple quand un membre d'un réseau de zombies (botnet) approche d'une des IP du cluster. En savoir plus sur les [clients suspects](#suspicious-clients).
   - **IP serveur suspectes (trafic sortant)** : Cette carte montre les indicateurs clés de performance et les résultats qui ont des [IP serveur suspectes](#suspicious-server-ips) s'exécutant dans le cadre du cluster. Exemple : Quand le cluster approche un serveur suspecté de distribuer des logiciels malveillants. 


## Collecte des données
{: #data-collection}

{{site.data.keyword.security-advisor_short}} peut vous aider à protéger votre cluster en ajoutant une surveillance réseau. Déployé dans le cadre de votre cluster, la surveillance réseau est utilisée pour fournir des informations sur les communications du cluster. Pour activer Network Analytics, les informations de flux réseau sont collectées pour la communication établie entre le cluster et des entités externes.
{: shortdesc}

Les informations suivantes sont collectées :

* L'adresse IP des homologues qui communiquent
* Les ports qu'ils utilisent
* L'ensemble des protocoles qui sont utilisés
* La quantité de données transférée
* Un ensemble de paramètres spécifiques au protocole
* Différents horodatages

**Les données réelles échangées ne sont pas collectées**.

La surveillance réseau fonctionne dans le cadre du cluster avec d'autres services comme {{site.data.keyword.loganalysisshort_notm}} afin de vous permettre de vous concentrer sur la logique métier. Vous pouvez examiner les analyses de la surveillance réseau dans votre tableau de bord {{site.data.keyword.security-advisor_short}}.


## Clients suspects (trafic entrant)
{: #suspicious-clients}

Le tableau de bord {{site.data.keyword.security-advisor_short}} inclut une carte de clients suspects qui récapitule différentes informations sur les communications dans lesquelles le cluster agit en tant que serveur approché par un client externe.
{: shortdesc}

Les communications analysées peuvent générer un résultat tel que : 

- Un client externe avec une mauvaise réputation, par exemple un lient connu pour distribuer des logiciels malveillants ou qui est associé à des services anonymes, approche l'une des URL ou des IP associées au cluster. Par exemple, un scanner approche l'un des ports ouverts du cluster. Ce type de communication peut suggérer une protection inadéquate des URL et IP associées au cluster, ainsi que des risques potentiels ou réels.


## IP serveur suspectes (trafic sortant)
{: #suspicious-server-ips}

Le tableau de bord {{site.data.keyword.security-advisor_short}} inclut une carte d'IP serveur suspectes qui récapitule différentes informations sur les communications dans lesquelles le cluster agit en tant que client qui approche un serveur externe.
{: shortdesc}

Les communications analysées peuvent générer un résultat tel que : 

- Un client au sein du cluster, tel qu'un déploiement de charge de travail, approche un noeud externe de mauvaise réputation, comme un botnet (réseau de zombies). Une communication de ce type peut suggérer que la charge de travail est compromise et requiert l'attention de votre équipe de sécurité. Le résultat varie en fonction de la réputation du noeud externe approché, des modèles de communication et du niveau de certitude proposés par l'intelligence. 

- L'URL du cluster ou l'une de ses IP possède une mauvaise réputation. Examinez cette réputation afin de vérifier que le cluster n'est pas compromis ni engagé dans une activité inacceptable. 
