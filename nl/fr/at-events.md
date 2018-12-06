---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-11"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Evénements {{site.data.keyword.cloudaccesstrailshort}}
{: #at_events}

Vous pouvez visualiser, gérer et auditer des activités initiées par l'utilisateur et effectuées dans votre instance de service {{site.data.keyword.security-advisor_long}} en utilisant le service {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

Pour plus d'informations sur le fonctionnement de ce service, voir les [documents sur {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker/index.html).


## Où visualiser les événements
{: #monitor}

Les événements sont disponibles dans le **domaine du compte** {{site.data.keyword.cloudaccesstrailshort}} qui est disponible dans la région {{site.data.keyword.Bluemix_notm}} où les événements sont générés.

1. Connectez-vous à votre compte {{site.data.keyword.Bluemix_notm}}.
2. Depuis le catalogue, mettez à disposition une instance du service {{site.data.keyword.cloudaccesstrailshort}} dans le même compte que votre instance {{site.data.keyword.security-advisor_long}}.
3. Dans l'onglet **Gérer** du tableau de bord {{site.data.keyword.cloudaccesstrailshort}}, cliquez sur **Afficher dans Kibana**.
4. Définissez la période pour laquelle vous désirez consulter les journaux. La valeur par défaut est 15 min.
5. Dans la liste **Zones disponibles**, cliquez sur **type**. Cliquez sur l'icône représentant une loupe pour qu'**Activity Tracker** limite les journaux uniquement à ceux dont le suivi est assuré par le service.
6. Vous pouvez utiliser les autres zones disponibles pour affiner la recherche.

Pour que les utilisateurs autres que le propriétaire du compte puissent afficher les journaux, vous devez utiliser le plan Premium. Pour laisser d'autres utilisateurs consulter des événements, voir [Octroi de droits de consultation des événements de compte](/docs/services/cloud-activity-tracker/how-to/grant_permissions.html#grant_permissions).
{: tip}

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
</table>
