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



# Evénements {{site.data.keyword.cloudaccesstrailshort}}
{: #at_events}

Vous pouvez visualiser, gérer et auditer des activités initiées par l'utilisateur et effectuées dans votre instance de service {{site.data.keyword.security-advisor_long}} en utilisant le service {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}






Pour plus d'informations sur le fonctionnement de ce service, voir la [documentation de {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started#getting-started).



## Où visualiser les événements
{: #monitor}

Les événements sont disponibles dans le **domaine du compte** {{site.data.keyword.cloudaccesstrailshort}} qui est disponible dans la région {{site.data.keyword.cloud_notm}} dans laquelle les événements sont générés.

1. Connectez-vous à votre compte {{site.data.keyword.cloud_notm}}.
2. Depuis le catalogue, mettez à disposition une instance du service {{site.data.keyword.cloudaccesstrailshort}} sur le même compte que votre instance de {{site.data.keyword.security-advisor_short}}.
3. Dans l'onglet **Gérer** du tableau de bord {{site.data.keyword.cloudaccesstrailshort}}, cliquez sur **Afficher dans Kibana**.
4. Définissez la période pour laquelle vous voulez consulter les journaux. La valeur par défaut est 15 minutes.
5. Dans la liste **Zones disponibles**, cliquez sur **type**. Cliquez sur l'icône représentant une loupe pour qu'**Activity Tracker** limite les journaux uniquement à ceux dont le suivi est assuré par le service.
6. Vous pouvez utiliser les autres zones disponibles pour affiner la recherche.



## Liste des événements
{: #events}

Consultez le tableau ci-dessous pour la liste des événements qui sont envoyés à {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

<table>
  <tr>
    <th>Action</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>Afficher une ou plusieurs notes précédemment créées.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>Créer une note.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>Supprimer une note.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>Mettre à jour une note.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>Afficher une ou plusieurs occurrences.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>Créer une occurrence.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>Supprimer une occurrence.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>Mettre à jour une occurrence.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>Lire les solutions personnalisées qui ont été ajoutées au service.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>Ajouter une solution personnalisée au service.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>Mettre à jour une solution personnalisée existante dans le service.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>Supprimer une solution personnalisée du service.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>Afficher les solutions partenaires que vous avez ajoutées à votre instance de service.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>Ajouter une solution partenaire à votre instance de service.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>Mettre à jour une solution partenaire dans votre instance de service.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>Supprimer une solution partenaire de votre instance de service.</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.enable</code></td>
    <td>Activez la fonction Network Insights mise à disposition par {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.disable</code></td>
    <td>Désactivez la fonction Network Insights mise à disposition par {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.enable</code></td>
    <td>Activez la fonction Activity Insights mise à disposition par {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.disable</code></td>
    <td>Activez la fonction Activity Insights mise à disposition par {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code>security-advisor.insights-cos.create</code></td>
    <td>Créez une instance de Cloud Object Storage par le biais de {{site.data.keyword.security-advisor_short}} pour les analyses de réseau et d'activité.</td>
  </tr>
</table>
