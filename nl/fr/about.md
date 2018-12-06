---

copyright:
  years: 2018
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

# A propos de {{site.data.keyword.security-advisor_short}}
{: #about}

{{site.data.keyword.security-advisor_long}} permet aux administrateurs de la sécurité de rechercher, définir les priorités et gérer les problèmes de sécurité dans leurs applications et charges de travail cloud.
{:shortdesc}

{{site.data.keyword.security-advisor_short}} centralise les services d'analyse tels que Vulnerability Advisor, et {{site.data.keyword.cloudcerts_short}} dans {{site.data.keyword.Bluemix_notm}}. Vous pouvez gérer les événements de sécurité et appliquer des analyses afin de générer des résultats permettant d'unifier et d'améliorer vos processus de gestion de la sécurité dans {{site.data.keyword.Bluemix_notm}}.

{{site.data.keyword.security-advisor_short}} propose également une fonction d'essai d'analyse réseau qui s'exécute sur votre cluster {{site.data.keyword.containerlong_notm}} afin de collecter les données de flux réseau permettant de détecter les communications avec des clients et IP serveur suspects.

## Concepts clés
{: #concepts}

Découvrez les différents concepts que vous pouvez utiliser lors de l'utilisation de {{site.data.keyword.security-advisor_short}}.
{: shortdesc}

<dl>
  <dt>Résultat</dt>
    <dd>Un résultat est un problème de sécurité avec priorité créé quand des événements bruts sont traités. Les résultats sont constitués d'éléments clé d'information nécessaires pour identifier les qui, quoi, quand et où du problème. En tant d'administrateur de la sécurité, vous pouvez utiliser les résultats {{site.data.keyword.security-advisor_short}} pour définir les priorités et réagir aux situations détectées. </dd>
  <dt>Indicateur clé de performance (KPI)</dt>
    <dd>Un indicateur clé de performance est déclenché quand la valeur d'un résultat dépasse les limites de la plage de performances acceptables pour des contrôles de sécurité spécifiques sur des services et des charges de travail.</dd>
  <dt>Note</dt>
    <dd>Vous pouvez créer des notes pour catégoriser les résultats rencontrés lorsque vous effectuez une analyse. Une note peut avoir leur plusieurs fois pour les différents fournisseurs.</dd>
  <dt>Occurrence</dt>
    <dd>Une occurrence décrit les détails d'une note qui sont spécifique au fournisseur. L'occurrence contient les détails de la vulnérabilité, des étapes de résolution, ainsi que d'autres informations générales.</dd>
  <dt>CRN du service</dt>
    <dd>Le CRN (nom de ressource de cloud) du service identifie le service {{site.data.keyword.Bluemix_notm}} impliqué dans le résultat.</br>
      <table>
        <tr>
          <th>Type de résultat</th>
          <th>Nom de ressource de cloud</th>
        </tr>
        <tr>
          <td>{{site.data.keyword.cloudcerts_short}}</td>
          <td>CRN de l'instance de service.</td>
        </tr>
        <tr>
          <td>Network Analytics</td>
          <td>CRN du cluster Kubernetes.</td>
        </tr>
        <tr>
          <td>Vulnerability Advisor</td>
          <td>CRN du conteneur.</td>
        </tr>
      </table></dd>
    <dt>CRN De la ressource</dt>
      <dd>Le CRN (nom de ressource de cloud) de la ressource identifie la ressource spécifique qui est impliquée dans le résultat.</dd>
</dl>
