---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-06"

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


# Network Insights (bêta)
{: #network}

Avec {{site.data.keyword.security-advisor_long}}, vous pouvez obtenir une analyse reposant sur le renseignement sur les menaces, des modèles comportementaux, et l'apprentissage automatique pour les conteneurs potentiellement compromis qui s'exécutent dans vos clusters {{site.data.keyword.containerlong_notm}}.
{: shortdesc}


## Fonctionnement
{: #network-how-it-works}

La fonction Network Insights est un module complémentaire  du service {{site.data.keyword.security-advisor_short}}. Une fois la fonction activée et configurée, les communications réseau du cluster, entrantes comme sortantes, entre votre cluster et des entités externes, sont collectées et surveillées et analysées en permanence.

Reportez-vous à l'image ci-dessous pour examiner le flux d'informations.

![Organigramme de Network Insights](images/network-insights-flow.png)

1. En tant qu'administrateur de compte, vous pouvez installer Network Insights dans chaque cluster à surveiller.
2. Les journaux de flot réseau sont réacheminés dans un compartiment Cloud Object Storage, dans lequel ils sont stockés jusqu'à ce que vous décidiez de les supprimer. Lorsque vous utilisez l'interface graphique de {{site.data.keyword.security-advisor_short}} pour créer le compartiment, des rôles IAM service à service sont affectés pour que le service puisse voir les journaux.
3. Lorsque Network Insights est activé, les données brutes qui se trouvent dans votre compartiment COS sont analysées pour identifier tout comportement suspect.
4. Lorsqu'un problème de sécurité potentiel est signalé, le résultat est réacheminé dans la base de données des résultats.
5. Les résultats sont affichés dans votre tableau de bord du service sur trois cartes :
  * La carte du trafic entrant suspect (Suspicious inbound traffic)
  * La carte du trafic sortant suspect (Suspicious outbound traffic)
  * La carte des anomalies de trafic (Anomalies in traffic)


## Collecte de données
{: #network-data}

{{site.data.keyword.security-advisor_short}} met à disposition un collecteur qui est utilisé pour regrouper les informations de flot réseau échangées entre un cluster et des entités externes.
{: shortdesc}

Les données brutes qui sont collectées sont stockées dans un compartiment Cloud Object Storage dans lequel vous pouvez déterminer la durée de stockage. Vous possédez et contrôlez les données collectées, ce qui signifie que vous êtes en charge de leur stockage, de leur sécurisation et de leur suppression. {{site.data.keyword.security-advisor_short}} conserve les résultats pendant 90 jours. Au cours de cette période, les résultats sont présentés sur la carte Network Insights dans le tableau de bord du service. Ainsi, même si vous ne voyez plus les résultats dans votre tableau de bord au bout de 90 jours, il se peut que les données brutes soient encore stockées.

Les informations suivantes sont collectées :

* L'adresse IP des homologues qui communiquent
* Les ports qu'ils utilisent
* L'ensemble des protocoles qui sont utilisés
* La quantité de données transférée
* Un ensemble de paramètres spécifiques au protocole
* Différents horodatages

Les données réelles échangées ne sont pas collectées.
{: tip}

Du point de vue de la sécurité, il est recommandé de purger vos données collectées lorsque des obligations légales ou d'entreprise permettent leur suppression. Pour plus d'informations, voir [Suppression d'objets](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion).


## Network: Suspicious inbound traffic
{: #network-suspicious-inbound}

{{site.data.keyword.security-advisor_short}} vous informe des tentatives effectuées par des clients externes d'interrogation et de compromission de clusters dans l'onglet **Suspicious Inbound Traffic** dans le tableau de bord du service.
{: shortdesc}


Les modèles comportementaux des clients qui sont considérés par IBM X-Force comme distribuant des logiciels malveillants utilisés comme scanners, dans le cadre d'un réseau de zombies, afin d'exploiter une cryptomonnaie, ou pour des services d'anonymisation, sont tous surveillés en permanence. Si ce type de client approche l'un des clusters que vous surveillez et présente un comportement préoccupant, Network Insights émet un résultat. 


La carte comporte deux nouveaux indicateurs clé de risques :

* Reconnaissance by suspicious clients (Reconnaissance par des clients suspects) : inclut des résultats liés aux clients qui accèdent au cluster.
* Abnormally large payloads sent by suspicious clients (Contenus anormalement volumineux envoyés par des clients suspects) : inclut des résultats liés aux volumes de données qui sont envoyés entre des clients et le cluster. Un contenu anormal correspond à tout élément inhabituel pour votre cluster.


Les résultats peuvent inclure des clients suspects qui :

* Envoient des quantités de données anormalement élevées dans le cluster.
* Envoient un nombre de demandes anormalement élevé à un service de cluster.
* Semblent cibler le cluster comme indiqué par le nombre de tentatives d'interrogation du cluster.



## Network: Suspicious outbound traffic
{: #network-suspicious-outbound}

{{site.data.keyword.security-advisor_short}} signale tout conteneur potentiellement compromis qui s'exécute dans vos clusters {{site.data.keyword.containershort_notm}} sur la carte **Suspicious Outbound Traffic** dans le tableau de bord du service.
{: shortdesc}

Le service surveille en permanence les modèles comportementaux des conteneurs accédant à des clients qui sont considérés par IBM X-Force comme distribuant des logiciels malveillants utilisés comme scanners, dans le cadre d'un réseau de zombies, afin d'exploiter une cryptomonnaie, ou pour des services d'anonymisation. Lorsqu'un conteneur dans un cluster surveillé approche les homologues suspects et présente un comportement préoccupant, Network Insights émet un résultat.

La carte comporte deux nouveaux indicateurs clé de risques :

* Outbound approaches to suspicious servers (Approches sortantes des serveurs suspects) : résultats liés à un cluster accédant aux serveurs.
* Abnormally large payloads exchanged with suspicious servers (Contenus anormalement volumineux échangés avec des serveurs suspects) : résultats liés aux volumes de données qui sont envoyés entre le cluster et les serveurs.


Les résultats peuvent inclure des conteneurs qui :

* Envoient des quantités de données anormalement élevées à un serveur suspect. 
* Extraient une quantité de données élevée depuis un serveur suspect. 
* Envoient un nombre de demandes élevé à un serveur suspect. 


## Network: Anomalies in traffic (expérimental)
{: #network-anomalies}

Avec cette fonction expérimentale, {{site.data.keyword.security-advisor_short}} surveille votre réseau pour identifier des modèles comportementaux. Une fois les modèles formés, tout comportement apparaissant comme anormal est signalé et récapitulé dans le tableau de bord du service sur la carte **Anomalies in traffic**.
{: shortdesc}

Le système crée un modèle de comportement de conteneur normal en surveillant les modèles comportementaux entre un conteneur et ses homologues. Lorsque le modèle est capturé, il est surveillé afin d'identifier tout changement spécifique. Si un changement préoccupant a lieu, Network Insights émet un résultat.

La carte comporte deux nouveaux indicateurs clé de risques :

* Higher than normal reconnaissance or data exchange activity (Activité de reconnaissance ou d'échange de données plus élevée que la normale) : résultats liés à des interactions anormales qui sont détectées entre le cluster et tout homologue externe.
* Outbound approach to a new server (Approche sortante d'un nouveau serveur) : résultats liés aux nouveaux serveurs détectés approchés par le cluster.

Un résultat peut inclure :  

* Des conteneurs qui approchent des serveurs qui n'ont jamais été approchés auparavant. 
* Des conteneurs qui envoient ou consomment une quantité de données plus élevée que la normale à ou depuis des homologues spécifiques. 
* Le niveau d'interrogation d'un conteneur particulier, qui a augmenté considérablement. 

Une fois le modèle développé, des écarts par rapport au modèle généré sont détectés, et lorsqu'un changement préoccupant a lieu, Network Insights publie un résultat dans le tableau de bord Security Advisor. Les résultats sont résumés dans la carte "Network: Anomalies in Traffic". La carte comporte deux nouveaux indicateurs clé de risques. L'indicateur clé de risques "Higher than normal reconnaissance or data exchange activity" compte les résultats qui sont liés à des interactions anormales détectées entre le cluster et des homologues externes. L'indicateur clé de risques 'Outbound approach to a new server' compte les résultats qui sont liés aux nouveaux serveurs détectés approchés par le cluster.  

## Etapes suivantes
{: #network-next}

Vous êtes prêt ? Lisez la rubrique relative à l'[activation de Network Insights](/docs/services/security-advisor?topic=security-advisor-setup-network).
