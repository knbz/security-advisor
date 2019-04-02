---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-13"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

---

{:new_window: target="_blank"}
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


# Angepasst
{: #setup_custom}

{{site.data.keyword.security-advisor_short}} ermöglicht Ihnen die Integration Ihrer vorhandenen angepassten Sicherheitstools, unabhängig davon, ob es sich um Open-Source-Services, Services für die angepassten Entwicklung oder um Services anderer Anbieter handelt. Durch Hinzufügen der URL zur Liste Ihrer Integrationen können Sie eine Direktverbindung von {{site.data.keyword.security-advisor_short}} zu dem anderen Tool erstellen. Mithilfe der APIs zum Konfigurieren von Key Risk Indicators (KRIs) können Sie auch Ergebnisse von Drittanbietern integrieren, um kritische Sicherheitsereignisse direkt in eine neue Karte im Dashboard einzubinden.
{: shortdesc}


## Direktverbindung hinzufügen
{: #setup-custom-gui}

Mit dem {{site.data.keyword.security-advisor_short}}-Dashboard können Sie Ihre Sicherheitstools ohne großen Aufwand überwachen.
{: shortdesc}

### Vorbereitende Schritte
{: #custom-before-gui}

Damit Sie die Integration hinzufügen können, müssen Sie bei dem Partner, den Sie integrieren möchten, über ein Konto verfügen.

{{site.data.keyword.security-advisor_short}} definiert Berechtigungsnachweise, die sich auf den Partnerservice beziehen, nicht als persistent. Unternehmensbenutzer müssen sich mithilfe von SAML sowohl bei der {{site.data.keyword.Bluemix_notm}}-Instanz als auch bei dem Geschäftspartner authentifizieren.
{: note}

### Verbindung konfigurieren
{: #custom-configure-connection}

1. Melden Sie sich bei Ihrem Sicherheitstool an und rufen Sie Ihre eindeutige URL ab.

2. Melden Sie sich über die Konsole bei {{site.data.keyword.cloud_notm}} an.

3. Klicken Sie auf **Angepasste Integrationen > Direktverbindung**. Eine Anzeige wird geöffnet.

  1. Geben Sie Ihrer Lösung einen Namen. Sie dürfen nur alphanumerische Zeichen, Leerzeichen und Bindestriche (-) im Namen verwenden.

  2. Geben Sie die URL für die Lösung im folgenden Format ein: `www.<website>.<domain>`.

  3. Laden Sie ein Symbol oder eine Grafik hoch, um das Tool darzustellen.

  4. Klicken Sie auf **Verbindung herstellen**, um die Konfiguration abzuschließen. {{site.data.keyword.security-advisor_short}} erstellt die für die Integration erforderlichen Artefakte, z. B. die Service-ID, den API-Schlüssel, die Konto-ID und die Metadaten. Die Rolle des Schreibberechtigten (`writer`) wird zugewiesen.



## Ergebnisse von Drittanbietern integrieren

Die APIs richten sich nach der Grafeas ähnlichen Spezifikation für Artefaktmetadaten zum Speichern, Abfragen und Abrufen der kritischen Metadaten für die von Ihren Sicherheitstools und -services gemeldeten Untersuchungsergebnisse.
{: #setup-custom-api}

### Vorbereitende Schritte
{: #custom-before-api}

Stellen Sie vor dem Integrieren von Ergebnissen Ihres Drittanbietertools sicher, dass die folgenden Voraussetzungen erfüllt sind.

1. Stellen Sie sicher, dass der Benutzer- oder Service-ID, die Sie verwenden, die [IAM-Rolle](https://cloud.ibm.com/iam#/users) **Manager** zugewiesen ist.

1. Melden Sie sich bei {{site.data.keyword.cloud_notm}} an.

  ```
  ibmcloud login
  ```
  {: codeblock}

2. Rufen Sie Ihre Konto-ID ab. Weitere Informationen zu Servicerollen finden Sie in der Beschreibung der [{{site.data.keyword.security-advisor_short}}-Zugriffsrichtlinien](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

3. Rufen Sie Ihr Identity and Access Management-Token (IAM-Token) ab. Das Token wird im Abschnitt `--header` jeder API-Anforderung verwendet.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  IAM-Token verfallen jeweils nach Ablauf von 60 Minuten. In [Neues Token automatisch abrufen](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey) erfahren Sie, wie Sie unter Verwendung eines API-Schlüssels neue Token automatisch abrufen können.
  {: tip}



### Ergebnisse und KRIs importieren
{: #custom-adding}

1. Registrieren Sie einen neuen Untersuchungsergebnistyp, indem Sie eine Anmerkung erstellen. Verwenden Sie zur Erstellung der Anmerkung die [API für Untersuchungsergebnisse](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote). Stellen Sie sicher, dass Sie eine eindeutige Anbieter-ID auswählen, um Ihr angepasstes Tool anzugeben. Wenn Sie den Prozess durch die Verwendung einer Service-ID automatisieren, können Sie die Service-ID als Ihre Provider-ID verwenden.

  Beispielanforderung:

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<Konto-ID>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <IAM-Token>" -H "Content-Type: application/json" -d "{ \"kind\": \"FINDING\", \"short_description\": \"Untersuchungsergebnis meines Sicherheitstools\", \"long_description\": \"Ergebnisse meines angepassten Sicherheitstools anzeigen\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-findings-type\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"Mein angepasstes Sicherheitstool\" }, \"finding\": { \"severity\": \"MEDIUM\", \"next_steps\": [ { \"title\": \"Finden Sie heraus, warum dies als Risiko gemeldet wird\" } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Symbol für weitere Informationen"/> Erläuterungen zu den Komponenten dieses Befehls </th>
    </thead>
    <tbody>
      <tr>
        <td><code>note_name</code></td>
        <td><code>&lt;Konto-ID&gt;/providers/my-custom-tool/notes/my-custom-tool-findings-type</code></td>
      </tr>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>remediation</code></td>
        <td>Die Maßnahmen, die zur Lösung des Problems ergriffen werden müssen.</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>Ihr angepasstes Sicherheitstool.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>Eine ID für den Typ des Untersuchungsergebnisses, den Ihr Sicherheitstool gefunden hat.</td>
      </tr>
      <tr>
        <td><code>context</code><ul><li><code>region</code></li><li><code>resource_id</code></li> <li><code>resource_name</code></li> <li><code>resource_type</code></li> <li><code>service_name</code></li></ul></td>
        <td></br><ul><li><code>Die Position, an der das Untersuchungsergebnis aufgetreten ist.</code></li><li><code>Die ID der betreffenden Ressource.</code></li> <li><code>Der Name der Ressource.</code></li> <li><code>Der Typ der Ressource.</code></li> <li><code>Der Name des Service.</code></li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>url</code></li></ul></td>
        <td></br><ul><li>Die Dringlichkeitsstufe, die das Untersuchungsergebnis darstellt.</li> <li>Die Maßnahmen, die zur Lösung des Problems ergriffen werden können.</li> <li>Eine URL, unter der sich die Details des Untersuchungsergebnisses befinden.</li></ul></td>
      </tr>
    </tbody>
  </table>

  Beispielantwort:

  ```
    {
    "author": {
      "account_id": "account id",
      "email": "email id",
      "id": "IBM ID",
      "kind": "user"
    },
    "context": {
      "account_id": "account id"
    },
    "create_time": "2018-09-04T10:57:56.913871Z",
    "create_timestamp": 1536058676914,
    "finding": {
      "next_steps": [
        {
          "title": "Finden Sie heraus, warum dies als Risiko gemeldet wird"
        }
      ],
      "severity": "MEDIUM"
    },
    "id": "my-custom-tool-findings-type",
    "kind": "FINDING",
    "long_description": "Ergebnisse meines angepassten Sicherheitstools anzeigen",
    "name": "<Konto-ID>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
    "provider_id": "my-custom-tool",
    "provider_name": "<Konto-ID>/providers/my-custom-tool",
    "reported_by": {
      "id": "my-custom-tool",
      "title": "Mein angepasstes Sicherheitstool"
    },
    "shared": true,
    "short_description": "Untersuchungsergebnis meines Sicherheitstools",
    "update_time": "2018-09-04T10:57:56.913890Z",
    "update_timestamp": 1536058676914,
    "update_week_date": "2018-W36-2"
  }
  ```
  {: screen}

  Achten Sie darauf, sich den Namen der Anmerkung (Note) zu merken, die als Teil der Antwort zurückgegeben wird. Im vorliegenden Beispiel ist dies der Wert `/providers/my-custom-tool/notes/my-custom-tool-findings-type`. Dieser Wert wird im nächsten Schritt verwendet.
  {: tip}

2. Posten Sie Ergebnisse als KRIs oder Ereignisse, die ansonsten auch als [Vorkommen](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence) bezeichnet werden.

  Für jede Karte können Sie zwei Key Risk Indicators (KRIs) definieren.
  {: note}

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<Konto-ID>/providers/my-custom-tool/occurrences" -H "accept: application/json" -H "Authorization: <IAM-Token>" -H "Replace-If-Exists: true" -H "Content-Type: application/json" -d "{ \"note_name\": \"<Konto-ID>/providers/my-custom-tool/notes/my-custom-tool-findings-type\", \"kind\": \"FINDING\", \"remediation\": \"Vorgehensweise zur Lösung\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-finding-1\", \"context\": { \"region\": \"location\", \"resource_id\": \"pluginId\", \"resource_name\": \"www.myapp.com\", \"resource_type\": \"worker\", \"service_name\": \"application\" }, \"finding\": { \"severity\": \"HIGH\", \"next_steps\": [{ \"url\": \"Details URL\" }] }}"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Symbol für weitere Informationen"/> Erläuterungen zu den Komponenten dieses Befehls </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>Eine Kurzbeschreibung, in der das Untersuchungsergebnis in ein paar Worten zusammengefasst wird.</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>Eine längere Beschreibung mit mehr Details zu dem Untersuchungsergebnis.</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>Ihr angepasstes Sicherheitstool.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>Eine ID für den Typ des Untersuchungsergebnisses, den Ihr Sicherheitstool gefunden hat.</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>Die ID des Sicherheitstools, das das Untersuchungsergebnis gemeldet hat.</li><li>Der Titel des Sicherheitstools, das das Untersuchungsergebnis gemeldet hat.</li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>title</code></li></ul></td>
        <td></br><ul><li>Die Dringlichkeitsstufe, die das Untersuchungsergebnis darstellt.</li> <li>Die Maßnahmen, die zur Lösung des Problems ergriffen werden können.</li> <li>Der Titel des Untersuchungsergebnisses.</li></ul></td>
      </tr>
    </tbody>
  </table>

  Beispielantwort:

  ```
    {
    "author": {
      "account_id": "account id",
      "email": "email id ",
      "id": "user id",
      "kind": "user"
    },
    "context": {
      "account_id": "account id",
      "region": "location",
      "resource_id": "pluginId",
      "resource_name": "www.myapp.com",
      "resource_type": "worker",
      "service_name": "application"
    },
    "create_time": "2018-09-04T11:32:14.564809Z",
    "create_timestamp": 1536060734565,
    "finding": {
      "next_steps": [
        {
          "title": "Finden Sie heraus, warum dies als Risiko gemeldet wird",
          "url": "Details URL"
        }
      ],
      "severity": "HIGH"
    },
    "id": "my-custom-tool-finding-1",
    "kind": "FINDING",
    "long_description": "Ergebnisse meines angepassten Sicherheitstools anzeigen",
    "name": "<Konto-ID>/providers/my-custom-tool/occurrences/my-custom-tool-finding-1",
    "note_name": "<Konto-ID>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
    "provider_id": "my-custom-tool",
    "provider_name": "<Konto-ID>/providers/my-custom-tool",
    "remediation": "Vorgehensweise zur Lösung",
    "reported_by": {
      "id": "my-custom-tool",
      "title": "Mein angepasstes Sicherheitstool"
    },
    "short_description": "Untersuchungsergebnis meines Sicherheitstools",
    "update_time": "2018-09-04T11:32:14.564828Z",
    "update_timestamp": 1536060734565,
    "update_week_date": "2018-W36-2"
  }
  ```
  {: screen}

3. Definieren Sie, auf welcher Karte im Dashboard Ihr Untersuchungsergebnis angezeigt werden soll, indem Sie eine [Anmerkung](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote) erstellen.

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<Konto-ID>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <IAM-Token>" -H "Content-Type: application/json" -d "{ \"kind\": \"CARD\", \"provider_id\": \"my-custom-tool\", \"id\": \"custom-tool-card\", \"short_description\": \"Von meinem angepassten Tool gefundenes Sicherheitsrisiko\", \"long_description\": \"Details dazu, warum dieses Sicherheitsrisiko beseitigt werden muss\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"Mein Sicherheitstool\" }, \"card\": { \"section\": \"Meine Sicherheitstools\", \"title\": \"Untersuchungsergebnisse meines Sicherheitstools\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ], \"elements\": [ { \"kind\": \"NUMERIC\", \"text\": \"Anzahl der von meinem Sicherheitstool gemeldeten Untersuchungsergebnisse\", \"default_time_range\": \"1d\", \"value_type\": { \"kind\": \"FINDING_COUNT\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ] } } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Symbol für weitere Informationen"/> Erläuterungen zu den Komponenten dieses Befehls </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>CARD</code></td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>Ihr angepasstes Sicherheitstool.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>Eine ID für den Typ des Untersuchungsergebnisses, den Ihr Sicherheitstool gefunden hat.</td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>Eine Beschreibung, in der das Untersuchungsergebnis in ein paar Worten zusammengefasst wird.</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>Eine längere Beschreibung mit mehr Details zu dem Untersuchungsergebnis.</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>Die ID des Sicherheitstools, das das Untersuchungsergebnis gemeldet hat.</li><li>Der Titel des Sicherheitstools, das das Untersuchungsergebnis gemeldet hat.</li></ul></td>
      </tr>
      <tr>
        <td><code>card</code> <ul><li><code>section</code></li> <li><code>title</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>Der Abschnitt, in den die Karte passt.</li> <li>Der Titel der Karte.</li> <li><code>providers/<Provider-ID>/notes/my-custom-tool-findings-type</code></li></ul></td>
      </tr>
      <tr>
        <td><code>elements</code> <ul><li><code>kind</code></li> <li><code>text</code></li> <li><code>default_time_range</code></li></ul></td>
        <td></br><ul><li>Der Typ des Elements.</li> <li>Der Text, der angezeigt werden soll.</li> <li>Der Zeitraum, der überprüft werden soll.</li></ul></td>
      </tr>
      <tr>
        <td><code>value_type</code> <ul><li><code>kind</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>Der Typ des Werts.</li> <li>Der Name der Untersuchungsergebnisse, der auf der Karte angezeigt werden soll.</li></ul></td>
      </tr>
    </tbody>
  </table>

  Beispielantwort:
  ```
    {
    "author": {
      "account_id": "<account id",
      "email": "email id",
      "id": "user id",
      "kind": "user"
    },
    "card": {
      "elements": [
        {
          "default_time_range": "1d",
          "kind": "NUMERIC",
          "text": "Anzahl der von meinem Sicherheitstool gemeldeten Untersuchungsergebnisse",
          "value_type": {
            "finding_note_names": [
              "providers/my-custom-tool/notes/my-custom-tool-findings-type"
            ],
            "kind": "FINDING_COUNT"
          }
        }
      ],
      "finding_note_names": [
        "providers/my-custom-tool/notes/my-custom-tool-findings-type"
      ],
      "section": "Meine Sicherheitstools",
      "title": "Untersuchungsergebnisse meines Sicherheitstools"
    },
    "context": {
      "account_id": "<Konto-ID>"
    },
    "create_time": "2018-09-04T11:49:36.056047Z",
    "create_timestamp": 1536061776056,
    "id": "custom-tool-card",
    "kind": "CARD",
    "long_description": "Details dazu, warum dieses Sicherheitsrisiko beseitigt werden muss",
    "name": "<Konto-ID>/providers/my-custom-tool/notes/custom-tool-card",
    "provider_id": "my-custom-tool",
    "provider_name": "<Konto-ID>/providers/my-custom-tool",
    "reported_by": {
      "id": "my-custom-tool",
      "title": "Mein Sicherheitstool"
    },
    "shared": true,
    "short_description": "Von meinem angepassten Tool gefundenes Sicherheitsrisiko",
    "update_time": "2018-09-04T11:49:36.056066Z",
    "update_timestamp": 1536061776056,
    "update_week_date": "2018-W36-2"
  }
  ```
  {: screen}

4. Navigieren Sie zu Ihrem Service-Dashboard, um die von Ihnen erstellte Karte anzuzeigen.

## Beispielverwendung
{: #custom-example}

Nehmen Sie an, dass eine Ihrer Anwendungen in einem {{site.data.keyword.containershort_notm}}-Cluster mit dem Namen `cloudkingdom` ausgeführt wird. Abhängig von der Größe Ihrer Anwendung können sich mehrere Pods in Ihrem Cluster befinden, die alle gleichzeitig überwacht werden sollen. Sie könnten zum Beispiel über mehrere angepasste Tools verfügen, die Ihren Cluster auf verschiedene Bedrohungen überwachen und diese erkennen. Wenn einer Ihrer Pods im Cluster anfängt, ein ungewöhnliches Datenvolumen an externe Server zu senden, sollten Sie dies so schnell wie möglich erfahren. Das angepasste Tool, das die Datenübertragung überwacht, kann dieses Überwachungsergebnis finden und an {{site.data.keyword.security-advisor_short}} senden. Sollte eine weitere Ihrer angepassten Integrationen ein Problem erkennen, würde diese das Ergebnis ebenfalls an {{site.data.keyword.security-advisor_short}} senden. {{site.data.keyword.security-advisor_short}} zeigt dann die Ergebnisse aller Ihrer Überwachungswerkzeuge in einem einzigen Dashboard an. Dort können Sie sich rasch einen Überblick über alle Alerts verschaffen, ein Problem untersuchen und erfahren, welche Korrekturmaßnahmen Sie ergreifen können.


Beispielnutzdaten:

```
{
	"note_name": "<Konto-ID>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
	"kind": "FINDING",
	"remediation": "Vorgehensweise zur Beseitigung der Bedrohung durch Datenleck",
	"provider_id": "my-custom-tool",
	"id": "my-custom-tool-finding-2",
	"context": {
		"region": "location",
		"resource_id": "cluster crn",
		"resource_name": "cloudkingdom",
		"resource_type": "container",
		"service_name": "kubernetes service"
	},
	"finding": {
		"severity": "HIGH",
		"next_steps": [{
			"title": "Überprüfen Sie, welche Prozesse in Ihrem Cluster gerade ausgeführt werden. Wenn Sie vermuten, dass einer Ihrer Pods gehackt wurde, starten Sie ihn erneut und suchen Sie nach Sicherheitslücken des Image.",
                        "url":"https://console.bluemix.net/containers-kubernetes/clusters"
		}],
                "short_description": "Einer der Pods in Ihrem Cluster scheint übermäßig viele Daten zu verlieren.",
                "long_description": "Einer der Pods in Ihrem Cluster nimmt Kontakt mit externen Servern auf und sendet ihnen Daten mit einem Volumen, das das normale Verhalten des Pods überschreitet."
	}
}
```
{: screen}
