---

copyright:
  years: 2017, 2018
lastupdated: "2018-12-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Gestion de l'accès au service
{: #service-access}

En tant que propriétaire de compte, vous pouvez gérer l'accès aux instances d'{{site.data.keyword.security-advisor_long}} en utilisant {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). En définissant dans votre compte des politiques qui créent différents niveaux d'accès pour différents utilisateurs, vous pouvez garantir que chaque instance de {{site.data.keyword.security-advisor_short}} est sécurisée.
{: shortdesc}

Pour plus d'informations sur IAM, voir [Accès à IAM](/docs/iam/users_roles.html).

## Règles d'accès {{site.data.keyword.security-advisor_short}}
{: #access}

A chaque utilisateur qui accède à une instance du service {{site.data.keyword.security-advisor_short}} dans votre compte doit être affectée une politique d'accès avec un rôle utilisateur IAM défini. La politique détermine les actions qu'un utilisateur peut effectuer dans le contexte de cette instance de service spécifique.
{: shortdesc}

Les actions sont personnalisées et définies par le service {{site.data.keyword.Bluemix_notm}} en tant qu'opérations dont l'exécution est autorisée dans le service. Les actions sont ensuite mappées aux rôles utilisateur du service IAM. Dans le tableau ci-dessous, les actions et les droits requis pour {{site.data.keyword.security-advisor_short}} sont mappés.

<table><caption>Quels rôles peuvent effectuer quelles actions ?</caption>
  <col width="40%">
  <col width="40%">
  <col width="20%">
  <tr>
    <th>Action</th>
    <th>Explication</th>
    <th>Rôle</th>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>Afficher les résultats (findings) du rapport de sécurité.</td>
    <td>Lecteur</br>Auteur</br>Gestionnaire</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>Générer les résultats (findings) du rapport de sécurité.</td>
    <td>Auteur</br>Gestionnaire</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>Supprimer les résultats (findings) du rapport de sécurité.</td>
    <td>Gestionnaire</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>Editer les résultats (findings) du rapport de sécurité.</td>
    <td>Auteur</br>Gestionnaire</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>Afficher les métadonnées.</td>
    <td>Lecteur</br>Auteur</br>Gestionnaire</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>Supprimer les métadonnées.</td>
    <td>Gestionnaire</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>Générer des métadonnées.</td>
    <td>Gestionnaire</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>Mettre à jour les métadonnées.</td>
    <td>Gestionnaire</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>Lire les solutions personnalisés qui ont été ajoutées au service.</td>
    <td>Lecteur</br>Auteur</br>Gestionnaire</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>Ajouter une solution personnalisée au service.</td>
    <td>Gestionnaire</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>Mettre à jour une solution personnalisée existante au sein du service.</td>
    <td>Gestionnaire</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>Supprimer une solution personnalisée du service.</td>
    <td>Gestionnaire</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>Afficher les solutions partenaires que vous avez ajoutées à votre instance de service.</td>
    <td>Lecteur</br>Auteur</br>Gestionnaire</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>Ajouter une solution partenaire à votre instance de service.</td>
    <td>Gestionnaire</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>Mettre à jour une solution partenaire dans votre instance de service.</td>
    <td>Gestionnaire</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>Supprimer une solution partenaire de votre instance de service.</td>
    <td>Gestionnaire</td>
  </tr>
</table>

## Mappages d'API
{: #api-map}

Comment ces rôles correspondent-ils à l'API ? Consultez la table suivante pour voir comment les actions du service sont mappées à l'API.


| Méthode | Noeud final                                                                  |  Action du service                  |
|--------|---------------------------------------------------------------------------|----------------------------------|
| POST   | /v1/{account_id}/graph                                                    | security-advisor.findings.read   |
| POST   | /v1/{account_id}/providers/{provider_id}/notes                            | security-advisor.metadata.write  |
| GET    | /v1/{account_id}/providers/{provider_id}/notes                            | security-advisor.metadata.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.read   |
| PUT    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.update |
| DELETE | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.delete |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}/note | security-advisor.findings.read   |
| POST   | /v1/{account_id}/providers/{provider_id}/occurrences                      | security-advisor.findings.write  |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences                      | security-advisor.findings.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}/occurrences      | security-advisor.findings.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.read   |
| PUT    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.update |
| DELETE | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.delete |
