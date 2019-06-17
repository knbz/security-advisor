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


# Angepasste Ergebnisse
{: #setup_custom}

Mit {{site.data.keyword.security-advisor_short}} können Sie Ihre vorhandenen angepassten Sicherheitstools integrieren, unabhängig davon, ob es sich um Open-Source-Services, Services für die angepassten Entwicklung oder um Services anderer Anbieter handelt. Durch die Integration von Drittanbieterergebnissen ist es Ihnen möglich, alle Sicherheitstools und Ergebnisse in einem einzigen Dashboard zu konsolidieren, in dem Sie kritische Sicherheitsereignisse überwachen können.
{: shortdesc}


## Vorbereitende Schritte
{: #custom-before-api}

Stellen Sie vor dem Integrieren von Ergebnissen Ihres Drittanbietertools sicher, dass die folgenden Voraussetzungen erfüllt sind.

1. Stellen Sie sicher, dass der Benutzer- oder Service-ID, die Sie verwenden, die [IAM-Rolle](https://cloud.ibm.com/iam#/users) **Manager** zugewiesen ist.

2. Melden Sie sich bei {{site.data.keyword.cloud_notm}} an.

  ```
  ibmcloud login
  ```
  {: codeblock}

3. Rufen Sie Ihre Konto-ID ab. Weitere Informationen zu Servicerollen finden Sie in der Beschreibung der [{{site.data.keyword.security-advisor_short}}-Zugriffsrichtlinien](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

4. Rufen Sie Ihr Identity and Access Management-Token (IAM-Token) ab. Das Token wird im Abschnitt `--header` jeder API-Anforderung verwendet.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  IAM-Token verfallen jeweils nach Ablauf von 60 Minuten. In [Neues Token automatisch abrufen](/docs/iam?topic=iam-iamtoken_from_apikey) erfahren Sie, wie Sie unter Verwendung eines API-Schlüssels neue Token automatisch abrufen können.
  {: tip}



## Ergebnisse und KRIs importieren
{: #custom-adding}

Die APIs richten sich nach der Grafeas ähnlichen Spezifikation für Artefaktmetadaten zum Speichern, Abfragen und Abrufen der kritischen Metadaten für die von Ihren Sicherheitstools und -services gemeldeten Untersuchungsergebnisse. Die Integration kann mithilfe der APIs zur Konfiguration von Key Risk Indicators (KRIs) durchgeführt werden.
{: shortdesc}


### Schritt 1: Neuen Ergebnistyp registrieren
{: #custom-register-finding}

Um einen neuen Typ von Ergebnissen zu registrieren, können Sie eine Anmerkung erstellen. Zur Erstellung der Anmerkung können Sie die [API für Untersuchungsergebnisse](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external} verwenden. Stellen Sie sicher, dass Sie eine eindeutige Anbieter-ID auswählen, um Ihr angepasstes Tool anzugeben. Sie können den Prozess durch die Verwendung Ihrer Service-ID als Provider-ID automatisieren.

Beispielanforderung:

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<Konto-ID>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM-Token>" -H  "Content-Type: application/json" -d "{  \"kind\": \"FINDING\",  \"short_description\": \"Untersuchungsergebnis meines Sicherheitstools\",  \"long_description\": \"Ausführliche Beschreibung der Untersuchungsergebnisse des Sicherheitstools.\",  \"provder_id\": \"my-custom-tool\",  \"id\": \"my-custom-tool-findings-type\",  \"reported_by\": {    \"id\": \"my-custom-tool\",    \"title\": \"Angepasstes Sicherheitstool\"  } ,  \"finding\": {    \"severity\": \"MEDIUM\",    \"next_steps\": [      {        \"title\": \"Erläutern, warum dies als Risiko gemeldet wird.\"      }    ]  }}"
```
{: codeblock}

| Allgemein | Beschreibung | 
|:-----------------|:-----------------|
| `note_name` | `<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type`  |
| `kind` | `FINDING` |
| `remediation` | Die Maßnahmen, die zur Lösung des Problems ergriffen werden müssen. |
| `provider_id` | Ihr angepasstes Sicherheitstool. |
| `id` | Eine ID für den Typ des Untersuchungsergebnisses, den Ihr Sicherheitstool gefunden hat. |
{: class="simple-tab-table"}
{: caption="Tabelle 1. Erläuterung der Komponenten des Typs 'Allgemein' für den Befehl" caption-side="top"}
{: #registerfindingtable1}
{: tab-title="General"}
{: tab-group="register"}

| Kontext | Beschreibung | 
|:-----------------|:-----------------|
| `region` | Der Standort, an dem das Untersuchungsergebnis aufgetreten ist.  |
| `resource_id` | Die ID für die Ressource. |
| `resource_name` | Der Name der Ressource. |
| `resource_type` | Der Typ der Ressource. |
| `service_name` | Der Name des Service. |
{: class="simple-tab-table"}
{: caption="Tabelle 2. Erläuterung der Komponenten des Typs 'Kontext' für den Befehl" caption-side="top"}
{: #registerfinding2}
{: tab-title="Context"}
{: tab-group="register"}

| Untersuchungsergebnis | Beschreibung | 
|:-----------------|:-----------------|
| `severity` | Die Dringlichkeitsstufe, die das Untersuchungsergebnis darstellt.  |
| `next_steps` | Die Maßnahmen, die zur Lösung des Problems ergriffen werden können. |
| `url` | Eine URL, unter der sich die Details des Untersuchungsergebnisses befinden. |
{: class="simple-tab-table"}
{: caption="Tabelle 3. Erläuterung der Komponenten des Typs 'Untersuchungsergebnis' für den Befehl" caption-side="top"}
{: #registerfinding3}
{: tab-title="Finding"}
{: tab-group="register"}

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


### Schritt 2: Ergebnisse posten
{: #custom-post-findings}

Erstellen Sie ein [Vorkommen](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence){: external}, um Untersuchungsergebnisse als KRIs oder Ereignisse in Ihrem Sicherheitsadvisordashboard zu posten.

Für jede Karte können Sie zwei Key Risk Indicators (KRIs) definieren.
{: note}

Beispielanforderung: 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<Konto-ID>/providers/my-custom-tool/occurrences"  -H  "accept: application/json" -H  "Authorization: <IAM-Token>" -H  "Content-Type: application/json" -d "{    \"note_name\": \"<Konto-ID>/providers/my-custom-tool/notes/my-custom-tool-findings-type\",    \"kind\": \"FINDING\",    \"remediation\": \"Beheben eines Sicherheitsrisikos aufgrund eines Datenlecks\",    \"provider_id\": \"my-custom-tool\",    \"id\": \"my-custom-tool-finding-2\",    \"context\": {        \"region\": \"location\",        \"resource_id\": \"cluster crn\",        \"resource_name\": \"cloudkingdom\",        \"resource_type\": \"container\",        \"service_name\": \"kubernetes service\"    },    \"finding\": {        \"severity\": \"HIGH\",        \"next_steps\": [{            \"title\": \"Im Cluster ausgeführte Prozesse ermitteln. Wenn Sie vermuten, dass einer der Pods gehackt wurde, starten Sie ihn erneut und suchen Sie nach Sicherheitslücken des Images.\",                        \"url\":\"https://cloud.ibm.com/containers-kubernetes/clusters\"        }],                \"short_description\": \"Einer der Pods in Ihrem Cluster scheint ungewöhnlich viele Daten zu verlieren.\",                \"long_description\": \"Einer der Pods in Ihrem Cluster nimmt Kontakt mit externen Servern auf und sendet ihnen Daten mit einem Volumen, das das normale Verhalten des Pods überschreitet.\"    }}"
```
{: codeblock}

| Allgemein | Beschreibung | 
|:-----------------|:-----------------|
| `kind` | `FINDING` |
| `short_description` | Eine Kurzbeschreibung, in der das Untersuchungsergebnis in ein paar Worten zusammengefasst wird. |
| `long_description` | Eine längere Beschreibung mit mehr Details zu dem Untersuchungsergebnis. |
| `provider_id` | Ihr angepasstes Sicherheitstool. |
| `id` | Eine ID für den Typ des Untersuchungsergebnisses, den Ihr Sicherheitstool gefunden hat. |
{: class="simple-tab-table"}
{: caption="Tabelle 2. Erläuterung der Komponenten des Typs 'Allgemein' für den Befehl" caption-side="top"}
{: #postfinding1}
{: tab-title="General"}
{: tab-group="post"}

| Gemeldet durch | Beschreibung | 
|:-----------------|:-----------------|
| `id` | Die ID des Sicherheitstools, das das Untersuchungsergebnis gemeldet hat.  |
| `title` | Der Titel des Sicherheitstools, das das Untersuchungsergebnis gemeldet hat. |
{: class="simple-tab-table"}
{: caption="Tabelle 2. Erläuterung der Komponenten des Typs 'Gemeldet durch' für den Befehl" caption-side="top"}
{: #postfinding2}
{: tab-title="Reported by"}
{: tab-group="post"}

| Untersuchungsergebnis | Beschreibung | 
|:-----------------|:-----------------|
| `severity` | Die Dringlichkeitsstufe, die das Untersuchungsergebnis darstellt.  |
| `next_steps` | Die Maßnahmen, die zur Lösung des Problems ergriffen werden können. |
| `title` | Der Titel des Untersuchungsergebnisses. |
{: class="simple-tab-table"}
{: caption="Tabelle 3. Erläuterung der Komponenten des Typs 'Untersuchungsergebnis' für den Befehl" caption-side="top"}
{: #postfinding3}
{: tab-title="Finding"}
{: tab-group="post"}


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

### Schritt 3: Anzeigemodus der Karte definieren
{: #custom-define-card}

Definieren Sie, wie die Karte die Ergebnisse im Dashboard anzeigen soll, indem Sie eine [Anmerkung](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external} erstellen.

Beispielanforderung: 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<Konto-ID>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM-Token>" -H  "Content-Type: application/json" -d "{    \"kind\": \"CARD\",        \"provider_id\": \"my-custom-tool\",    \"id\": \"custom-tool-card\",    \"short_description\": \"Vom angepassten Tool festgestelltes Sicherheitsrisiko.\",        \"long_description\": \"Ausführlichere Beschreibung, warum dieses Sicherheitsrisiko behoben werden muss.",    \"reported_by\": {      \"id\": \"my-custom-tool\",      \"title\": \"Mein Sicherheitstool\"    },        \"card\": {      \"section\": \"Meine Sicherheitstools\",      \"order\": 1 ,     \"title\": \"Untersuchungsergebnisse meines Sicherheitstools\",      \"subtitle\": \"Mein Sicherheitstool\",      \"finding_note_names\": [        \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"      ],      \"elements\": [        {          \"kind\": \"NUMERIC\",          \"text\": \"Anzahl der von meinem Sicherheitstool gemeldeten Ergebnisse\",          \"default_time_range\": \"1d\",          \"value_type\": {            \"kind\": \"FINDING_COUNT\",            \"finding_note_names\": [              \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"            ]          }        }      ]    }  }"
```
{: codeblock}

| Allgemein | Beschreibung | 
|:-----------------|:-----------------|
| `provider_id` | Die ID Ihres Sicherheitstools. |
| `id` | Eine ID für den Typ des Untersuchungsergebnisses, den Ihr Sicherheitstool gefunden hat. |
| `short_description` | Eine Kurzbeschreibung, in der das Untersuchungsergebnis in ein paar Worten zusammengefasst wird. |
| `long_description` | Eine längere Beschreibung mit mehr Details zu dem Untersuchungsergebnis. |
{: class="simple-tab-table"}
{: caption="Tabelle 1. Erläuterung der Komponenten des Typs 'Allgemein' für den Befehl" caption-side="top"}
{: #definecard1}
{: tab-title="General"}
{: tab-group="card"}

| Gemeldet durch | Beschreibung | 
|:-----------------|:-----------------|
| `id` | Die ID des Sicherheitstools, das das Untersuchungsergebnis gemeldet hat.  |
| `title` | Der Titel des Sicherheitstools, das das Untersuchungsergebnis gemeldet hat. |
{: class="simple-tab-table"}
{: caption="Tabelle 2. Erläuterung der Komponenten des Typs 'Gemeldet durch' für den Befehl" caption-side="top"}
{: #definecard2}
{: tab-title="Reported by"}
{: tab-group="card"}

| Karte | Beschreibung | 
|:-----------------|:-----------------|
| `section` | Abschnitt, in dem die Karte angezeigt werden soll. Es sind bis zu drei angepasste Abschnitte mit jeweils sechs Karten möglich. Maximale Anzahl Zeichen: 30 |
| Optional: `order` | Reihenfolge, in der die Karten im angegebenen Abschnitt angezeigt werden. Die Reihenfolge wird mit den Zahlen 1 - 6 angegeben. Bei der Auswahl einer Zahl, die bereits für eine andere Karte angewendet wird, schlägt die Erstellung fehl. Sie erhalten die Fehlernachricht "Die angegebene Position in der Reihenfolge wird bereits für eine andere Karte im Abschnitt verwendet". Wenn die Angabe größer ist als die aktuelle Kartenanzahl plus 1, schlägt die Kartenerstellung fehl. Beispiel: Wenn 2 Karten vorhanden sind und Sie eine weitere erstellen, kann für die Position in der Reihenfolge nicht 5 angegeben werden, da insgesamt nur drei Karten vorhanden sind. Wird keine Reihenfolge für die Karten angegeben, werden sie im zugewiesenen Abschnitt alphabetisch aufgeführt. |
| `title` | Der gewünschte Titel für die Karte. Maximale Anzahl Zeichen: 28 |
| `subtitle` | Der gewünschte Untertitel für die Karte. Maximale Anzahl Zeichen: 30 |
| `finding_note_names` | `providers//notes/my-custom-tool-findings-type` |
{: class="simple-tab-table"}
{: caption="Tabelle 3. Erläuterung der Komponenten des Typs 'Karte' für den Befehl" caption-side="top"}
{: #definecard3}
{: tab-title="Card"}
{: tab-group="card"}

| Elemente: Werttyp | Beschreibung | 
|:-----------------|:-----------------|
| `kind` | Folgende Optionen sind möglich: `NUMERIC`, `TIME_SERIES` und `BREAKDOWN`. |
| `text` | Der Text, der angezeigt werden soll. Bei der Angabe `NUMERIC` beträgt die maximale Anzahl 60 Zeichen. Bei der Angabe `TIME_SERIES` oder `BREAKDOWN` beträgt die maximale Anzahl 65 Zeichen. |
| `default_time_range` | Der Zeitraum, der überprüft werden soll. Die Werte werden in Tagen festgelegt. Aktuelle Optionen: `1d`, `2d`, `3d` und `4d`. |
| `value_type` | Die Art des Elements. Bei der Angabe `NUMERIC` lautet das Feld `value_type` und es sind bis zu vier Elemente pro Karte möglich. Bei der Angabe `TIME_SERIES` oder `BREAKDOWN` lautet das Feld `value_types`. Die maximale Anzahl sowohl für `TIME_SERIES` als auch für `BREAKDOWN` ist 1. Wenn nur numerische Einträge vorhanden sind, sind bis zu vier Elemente pro Karte möglich. Wenn Sie eine Kombination verwenden möchten, sind bis zu zwei numerische Einträge und ein Eintrag des Typs 'Zeitreihe' oder 'Aufgliederung' möglich. Es ist nicht möglich, sowohl Einträge des Typs 'Zeitreihe' als auch Einträge des Typs 'Aufgliederung' auf derselben Karte zu verwenden. Wenn Sie die Werttypen als Array für Zeitreihen definieren, sind bis zu drei Einträge möglich. |
| `value_type`: `kind` | Der Typ des Werts. Folgende Optionen sind möglich: `KRI` und `FINDING_COUNT`. |
| `value_type`: `finding_note_names` | Wenn für `kind` die Angabe `FINDING_COUNT` festgelegt wird, ist dies der Name der Untersuchungsergebnisse für die Anzeige auf der Karte. Die Angabe erfolgt als Array. |
| `value_type`: `kri_note_names` | Wenn für `kind` die Angabe `FINDING_COUNT` festgelegt wird, ist dies der Name der Untersuchungsergebnisse für die Anzeige auf der Karte. Die Angabe erfolgt als Array. |
| `value_type`: `text` | Der Text des Elementtyps. Die maximale Anzahl von Zeichen beträgt 22. |
{: class="simple-tab-table"}
{: caption="Tabelle 4. Erläuterung der Komponenten des Typs 'Element' für den Befehl" caption-side="top"}
{: #definecard4}
{: tab-title="Elements"}
{: tab-group="card"}

Beispielantwort:

```
{
"author": {
  "account_id": "account id",
      "email": "email id ",
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
  "order": "1",
  "title": "Untersuchungsergebnisse meines Sicherheitstools",
  "subtitle": "Mein Sicherheitstool",
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
			"title": "Überprüfen Sie, welche Prozesse in Ihrem Cluster gerade ausgeführt werden. Wenn Sie vermuten, dass einer Ihrer Pods gehackt wurde, starten Sie ihn erneut und suchen Sie nach Sicherheitslücken des Images.",
                        "url":"https://cloud.ibm.com/containers-kubernetes/clusters"
		}],
                "short_description": "Einer der Pods in Ihrem Cluster scheint übermäßig viele Daten zu verlieren.",
                "long_description": "Einer der Pods in Ihrem Cluster nimmt Kontakt mit externen Servern auf und sendet ihnen Daten mit einem Volumen, das das normale Verhalten des Pods überschreitet."
	}
}
```
{: screen}
