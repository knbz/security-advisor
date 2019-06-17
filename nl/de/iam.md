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



# Servicezugriff verwalten
{: #service-access}

Als Kontoeigner können Sie den Zugriff auf Instanzen von {{site.data.keyword.security-advisor_long}} mithilfe von {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) steuern. Sie definieren Richtlinien in Ihrem Konto, die verschiedene Zugriffsebenen für verschiedene Benutzer erstellen, und können damit gewährleisten, dass jede Instanz von {{site.data.keyword.security-advisor_short}} sicher ist.
{: shortdesc}

Weitere Informationen zu IAM finden Sie unter [IAM-Zugriff](/docs/iam?topic=iam-userroles).

## {{site.data.keyword.security-advisor_short}}-Zugriffsrichtlinien
{: #access}

Jedem Benutzer, der auf eine Instanz des {{site.data.keyword.security-advisor_short}}-Service in Ihrem Konto zugreift, muss eine Zugriffsrichtlinie mit einer definierten IAM-Benutzerrolle zugeordnet werden. Die Richtlinie legt fest, welche Aktionen ein Benutzer im Kontext der jeweiligen Serviceinstanz ausführen kann.
{: shortdesc}

Die Aktionen werden angepasst und durch den {{site.data.keyword.cloud_notm}}-Service als Operationen definiert, deren Ausführung in dem Service zulässig ist. Dann werden die Aktionen IAM-Servicebenutzerrollen zugeordnet. Die folgende Tabelle enthält eine Auflistung der Aktionen und erforderlichen Berechtigungen für {{site.data.keyword.security-advisor_short}}.

<table><caption>Welche Rollen können welche Aktionen ausführen?</caption>
  <col width="40%">
  <col width="40%">
  <col width="20%">
  <tr>
    <th>Aktion</th>
    <th>Erläuterung</th>
    <th>Rolle</th>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>Untersuchungsergebnisse für Sicherheitsbericht anzeigen.</td>
    <td>Leser</br>Autor</br>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>Untersuchungsergebnisse für Sicherheitsbericht generieren.</td>
    <td>Autor</br>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>Untersuchungsergebnisse für Sicherheitsbericht löschen.</td>
    <td>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>Untersuchungsergebnisse für Sicherheitsbericht bearbeiten.</td>
    <td>Autor</br>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>Metadaten anzeigen.</td>
    <td>Leser</br>Autor</br>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>Metadaten löschen.</td>
    <td>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>Metadaten generieren.</td>
    <td>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>Metadaten aktualisieren.</td>
    <td>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>Angepasste Lösungen, die Sie zum Service hinzugefügt haben, lesen.</td>
    <td>Leser</br>Autor</br>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>Angepasste Lösung zum Service hinzufügen.</td>
    <td>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>Vorhandene angepasste Lösung im Service aktualisieren.</td>
    <td>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>Angepasste Lösung aus dem Service löschen.</td>
    <td>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>Partnerlösungen, die Sie zu Ihrer Serviceinstanz hinzugefügt haben, anzeigen.</td>
    <td>Leser</br>Autor</br>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>Partnerlösung zu Ihrer Serviceinstanz hinzufügen.</td>
    <td>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>Partnerlösung in Ihrer Serviceinstanz aktualisieren.</td>
    <td>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>Partnerlösung aus Ihrer Serviceinstanz löschen.</td>
    <td>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.enable</code></td>
    <td>Vom Service bereitgestelltes Network Insights-Feature aktivieren.</td>
    <td>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.disable</code></td>
    <td>Vom Service bereitgestelltes Network Insights-Feature inaktivieren.</td>
    <td>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.enable</code></td>
    <td>Vom Service bereitgestelltes Activity Insights-Feature aktivieren.</td>
    <td>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.disable</code></td>
    <td>Vom Service bereitgestelltes Activity Insights-Feature inaktivieren.</td>
    <td>Manager</td>
  </tr>
  <tr>
    <td><code>security-advisor.insights-cos.create</code></td>
    <td>Für Network Insights und Activity Insights über {{site.data.keyword.security-advisor_short}} eine Cloud Object Storage-Instanz erstellen.</td>
    <td>Manager</td>
  </tr>
</table>

## API-Zuordnungen
{: #api-map}

Welche Entsprechungen dieser Rollen gibt es in der API? Anhand der folgenden Tabelle können Sie feststellen, wie die Serviceaktionen der API zugeordnet werden.


| Methode | Endpunkt                                                                  |  Serviceaktion                  |
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
