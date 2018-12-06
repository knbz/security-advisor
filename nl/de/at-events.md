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


# {{site.data.keyword.cloudaccesstrailshort}}-Ereignisse
{: #at_events}

Mit dem {{site.data.keyword.cloudaccesstrailshort}}-Service können Sie vom Benutzer eingeleitete Aktivitäten in Ihrer {{site.data.keyword.security-advisor_long}}-Serviceinstanz anzeigen, verwalten und prüfen.
{: shortdesc}

Weitere Informationen zur Funktionsweise des Service finden Sie in der [{{site.data.keyword.cloudaccesstrailshort}}-Dokumentation](/docs/services/cloud-activity-tracker/index.html).


## Wo werden Ereignisse angezeigt?
{: #monitor}

Ereignisse sind in der {{site.data.keyword.cloudaccesstrailshort}}-**Kontodomäne** verfügbar, die zu der {{site.data.keyword.Bluemix_notm}}-Region gehört, in der die Ereignisse generiert werden.

1. Melden Sie sich bei Ihrem {{site.data.keyword.Bluemix_notm}}-Konto an.
2. Stellen Sie in dem Konto Ihrer {{site.data.keyword.security-advisor_long}}-Instanz eine Instanz des {{site.data.keyword.cloudaccesstrailshort}}-Service aus dem Katalog bereit.
3. Klicken Sie auf der Registerkarte **Verwalten** des {{site.data.keyword.cloudaccesstrailshort}}-Dashboards auf **In Kibana anzeigen**.
4. Legen Sie den Zeitrahmen fest, für den Sie Protokolle anzeigen wollen. Der Standardwert ist 15 Minuten.
5. Klicken Sie in der Liste der **verfügbaren Felder** auf **Typ**. Klicken Sie auf das Lupensymbol für **Activity Tracker**, um die Protokolle auf die Protokolle zu beschränken, die vom Service überwacht werden.
6. Sie können die Suche durch Angaben in den anderen verfügbaren Feldern eingrenzen.

Damit Benutzer, die nicht Kontoeigner sind, Protokolle anzeigen können, müssen Sie den Premium-Plan verwenden. Informationen zum Anzeigen von Ereignissen durch andere Benutzer finden Sie unter [Berechtigungen zum Anzeigen von Ereignissen erteilen](/docs/services/cloud-activity-tracker/how-to/grant_permissions.html#grant_permissions).
{: tip}

## Ereignisliste
{: #events}

Die folgende Tabelle enthält eine Auflistung der Ereignisse, die an {{site.data.keyword.cloudaccesstrailshort}} gesendet werden.
{: shortdesc}

<table>
  <tr>
    <th>Aktion</th>
    <th>Beschreibung</th>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>Zuvor erstellte Anmerkung(en) anzeigen.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>Anmerkung erstellen.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>Anmerkung löschen.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>Anmerkung aktualisieren.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>Vorkommen anzeigen.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>Vorkommen erstellen.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>Vorkommen löschen.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>Vorkommen aktualisieren.</td>
  </tr>
</table>
