---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Servicezugriff verwalten
{: #service-access}

Als Kontoeigner können Sie den Zugriff auf Instanzen von {{site.data.keyword.security-advisor_long}} mithilfe von {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) steuern. Sie definieren Richtlinien in Ihrem Konto, die verschiedene Zugriffsebenen für verschiedene Benutzer erstellen, und können damit gewährleisten, dass jede Instanz von {{site.data.keyword.security-advisor_short}} sicher ist.
{: shortdesc}

Weitere Informationen zu IAM finden Sie unter [IAM-Zugriff](/docs/iam/users_roles.html).

## {{site.data.keyword.security-advisor_short}}-Zugriffsrichtlinien
{: #access}

Jedem Benutzer, der auf eine Instanz des {{site.data.keyword.security-advisor_short}}-Service in Ihrem Konto zugreift, muss eine Zugriffsrichtlinie mit einer definierten IAM-Benutzerrolle zugeordnet werden. Die Richtlinie legt fest, welche Aktionen ein Benutzer im Kontext der jeweiligen Serviceinstanz ausführen kann.
{: shortdesc}

Die Aktionen werden angepasst und durch den {{site.data.keyword.Bluemix_notm}}-Service als Operationen definiert, deren Ausführung in dem Service zulässig ist. Dann werden die Aktionen IAM-Servicebenutzerrollen zugeordnet. Die folgende Tabelle enthält eine Auflistung der Aktionen und erforderlichen Berechtigungen für {{site.data.keyword.security-advisor_short}}.

<table><caption>Welche Rollen können welche Aktionen ausführen?</caption>
  <col width="43%">
  <col width="42%">
  <col width="5%">
  <col width="5%">
  <col width="5%">
  <tr>
    <th>Aktion</th>
    <th>Erläuterung</th>
    <th>Leseberechtigter</th>
    <th>Schreibberechtigter</th>
    <th>Manager</th>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>Untersuchungsergebnisse für Sicherheitsbericht anzeigen.</td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>Untersuchungsergebnisse für Sicherheitsbericht generieren.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>Untersuchungsergebnisse für Sicherheitsbericht löschen.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>Untersuchungsergebnisse für Sicherheitsbericht bearbeiten.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>Metadaten anzeigen.</td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>Metadaten löschen.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>Metadaten generieren.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>Metadaten aktualisieren.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>Angepasste Lösungen lesen, die dem Service hinzugefügt wurden.</td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>Angepasste Lösung zum Service hinzufügen.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>Vorhandene angepasste Lösung im Service aktualisieren.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>Angepasste Lösung aus dem Service löschen.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Funktion verfügbar" style="width:32px;" /></td>
  </tr>
</table>

## API-Zuordnungen
{: #api-map}

Welche Entsprechungen dieser Rollen gibt es in der API? Anhand der folgenden Tabelle können Sie feststellen, wie die Serviceaktionen der API zugeordnet werden.


| Methode| Endpunkt                                                                  |  Serviceaktion                   |
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
