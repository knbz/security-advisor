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



# {{site.data.keyword.cloudaccesstrailshort}}-Ereignisse
{: #at_events}

Mit dem {{site.data.keyword.cloudaccesstrailshort}}-Service können Sie vom Benutzer eingeleitete Aktivitäten in Ihrer {{site.data.keyword.security-advisor_long}}-Serviceinstanz anzeigen, verwalten und prüfen.
{: shortdesc}






Weitere Informationen zur Funktionsweise des Service finden Sie in der [{{site.data.keyword.cloudaccesstrailshort}}-Dokumentation](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started#getting-started).



## Wo werden Ereignisse angezeigt?
{: #monitor}

Ereignisse sind in der {{site.data.keyword.cloudaccesstrailshort}}-**Kontodomäne** verfügbar, die zu der {{site.data.keyword.cloud_notm}}-Region gehört, in der die Ereignisse generiert werden.

1. Melden Sie sich bei Ihrem {{site.data.keyword.cloud_notm}}-Konto an.
2. Stellen Sie in dem Konto Ihrer {{site.data.keyword.security-advisor_short}}-Instanz aus dem Katalog eine Instanz des {{site.data.keyword.cloudaccesstrailshort}}-Service bereit.
3. Klicken Sie auf der Registerkarte **Verwalten** des {{site.data.keyword.cloudaccesstrailshort}}-Dashboards auf **In Kibana anzeigen**.
4. Legen Sie den Zeitrahmen fest, für den Sie Protokolle anzeigen wollen. Der Standardwert ist 15 Minuten.
5. Klicken Sie in der Liste der **verfügbaren Felder** auf **Typ**. Klicken Sie auf das Lupensymbol für **Activity Tracker**, um die Protokolle auf die Protokolle zu beschränken, die vom Service überwacht werden.
6. Sie können die Suche durch Angaben in den anderen verfügbaren Feldern eingrenzen.



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
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>Angepasste Lösungen lesen, die dem Service hinzugefügt wurden.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>Angepasste Lösung zum Service hinzufügen.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>Vorhandene angepasste Lösung im Service aktualisieren.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>Angepasste Lösung aus dem Service löschen.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>Partnerlösungen anzeigen, die Sie Ihrer Serviceinstanz hinzugefügt haben.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>Partnerlösung zu Ihrer Serviceinstanz hinzufügen.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>Partnerlösung in Ihrer Serviceinstanz aktualisieren.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>Partnerlösung aus Ihrer Serviceinstanz löschen.</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.enable</code></td>
    <td>Das von {{site.data.keyword.security-advisor_short}} bereitgestellte Network Insights-Feature aktivieren.</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.disable</code></td>
    <td>Das von {{site.data.keyword.security-advisor_short}} bereitgestellte Network Insights-Feature inaktivieren.</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.enable</code></td>
    <td>Das von {{site.data.keyword.security-advisor_short}} bereitgestellte Activity Insights-Feature aktivieren.</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.disable</code></td>
    <td>Das von {{site.data.keyword.security-advisor_short}} bereitgestellte Activity Insights-Feature inaktivieren.</td>
  </tr>
  <tr>
    <td><code>security-advisor.insights-cos.create</code></td>
    <td>Für Network Insights und Activity Insights über {{site.data.keyword.security-advisor_short}} eine Cloud Object Storage-Instanz erstellen.</td>
  </tr>
</table>
