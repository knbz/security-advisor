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


# Activity Insights (Vorschau)
{: #activity}

Mit {{site.data.keyword.security-advisor_long}} können Sie verdächtige Benutzeraktivitäten in Ihrem {{site.data.keyword.cloud_notm}}-Konto mithilfe von {{site.data.keyword.cloud_notm}} Activity Tracker erkennen.
{: shortdesc}


## Funktionsweise
{: #activity-how}

Das Activity Insights-Feature ist ein Add-on für den {{site.data.keyword.security-advisor_short}}-Service. Wenn das Feature aktiviert und konfiguriert ist, wird das Benutzerverhalten protokolliert und analysiert, um verdächtige Aktivitäten auf der Grundlage von Regeln aufzudecken. Sie können Standardregeln verwenden oder aber angepasste Regeln erstellen, die den Bedürfnissen Ihrer Organisation entsprechen.

Die folgende Abbildung enthält eine Darstellung des Informationsflusses.

![Activity Insights-Flussdiagramm](images/activity-insights-flow.png)

1. Als Kontoadministrator können Sie Activity Insights in Ihrem Cluster installieren.
2. Wenn das Add-on in einem Cluster installiert ist, kann es die Protokolle von Activity Tracker für Ihr gesamtes Konto überwachen.
3. Die Aktivitätsprotokolle werden an ein Cloud Object Storage-Bucket weitergeleitet, in dem sie gespeichert werden, bis Sie beschließen, sie zu löschen. Wenn Sie die Benutzerschnittstelle (GUI) von {{site.data.keyword.security-advisor_short}} zum Erstellen des Buckets verwenden, werden Service-zu-Service-IAM-Rollen zugewiesen, sodass der Service die Protokolle anzeigen kann.
4. Wenn Activity Insights aktiviert ist, werden die Rohdaten in Ihrem COS-Bucket nach Regeln analysiert, die vom Service vordefiniert sein oder von Ihnen angepasst werden können.
5. Wenn ein mögliches Sicherheitsproblem markiert wird, wird das Ergebnis an die Datenbank für Untersuchungsergebnisse weitergeleitet.
6. Untersuchungsergebnisse werden in Ihrem Service-Dashboard auf der Karte **Activity Insights** angezeigt.

</br>

## Daten erfassen
{: #activity-data}

Activity Tracker sammelt Ereignisse, die Benutzerinteraktionen mit {{site.data.keyword.cloud_notm}}-APIs beschreiben. Die Protokolle können Sie dann zwecks weiterer Analyse in einem Object Storage-Bucket speichern.
{: shortdesc}

Activity Tracker sammelt Ereignisse, die Benutzerinteraktionen mit {{site.data.keyword.cloud_notm}}-APIs beschreiben.

Die erfassten Informationen umfassen unter anderem die folgenden Angaben:

* Die IP-Adresse des Initiators des API-Aufrufs
* Den Benutzer, der authentifiziert wurde
* Den Aktivitätstyp
* Das Ergebnis der Aktivität
* Weitere Angaben ...

Die erfassten Rohdaten werden in einem Cloud Object Storage-Bucket gespeichert, für den Sie die Speicherdauer bestimmen können. Sie sind der Eigner der erfassten Daten und Ihnen obliegt die Kontrolle über sie, was bedeutet, dass Sie für ihre Speicherung, Sicherung und Löschung verantwortlich sind. {{site.data.keyword.security-advisor_short}} bewahrt Untersuchungsergebnisse 90 Tage lang auf. Während dieser Zeit werden die Ergebnisse im Service-Dashboard auf der Karte **Activity Insights** dargestellt. Obwohl das Ergebnis also nach 90 Tagen nicht mehr in Ihrem Dashboard angezeigt wird, ist es durchaus möglich, dass sich die Rohdaten noch im Speicher befinden.

Aus dem Blickwinkel der Sicherheit ist es in der Regel sinnvoll, die erfassten Daten zu bereinigen, wenn gesetzliche oder betriebliche Voraussetzungen das Löschen zulassen. Weitere Informationen finden Sie in [Objekte löschen](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion).
{: tip}

## Activity Insights: Zugriff
{: #ai-access}

Die Activity Insights-Karte im Service-Dashboard fasst alle Meldungen unerwarteter oder alarmierender Kontoaktivitäten Ihrer Benutzer und Service zusammen. Außergewöhnliche Aktivitäten können ein Fehlverhalten legitimer Benutzer und Services darstellen oder aber ein Anzeichen dafür sein, dass Ihr Konto beeinträchtigt ist. Die Ergebnisse, die auf der Karte angezeigt werden, werden basierend auf den von {{site.data.keyword.security-advisor_short}} bereitgestellten Standard-Regelpaketen ermittelt.

Auf dieser Karte werden zwei Key Risk Indicators (KRIs) eingeführt:

* Identität und Zugriff: Ergebnisse, die sich auf IAM-Services (IAM - Identity and Access Management) oder {{site.data.keyword.appid_short_notm}}-Services beziehen.
* Daten und Kubernetes: Ergebnisse, die sich auf Key Protect, Kubernetes Service, Cloud Object Storage oder Certificate Manager beziehen.


## Regelpakete verstehen
{: #activity-packages}

Als Kontoadministrator können Sie rasch mit der Überwachung Ihrer Konten beginnen, indem Sie sich die Vorteile von Regelpaketen zunutze machen.
{: shortdesc}

Der Service bietet Regelpakete an, die mit mehreren Services verbunden sind. Dazu gehören unter anderem die folgenden:

* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)
* {{site.data.keyword.cloudcerts_long_notm}}
* {{site.data.keyword.appid_long_notm}}
* {{site.data.keyword.keymanagementservicelong_notm}}
* {{site.data.keyword.cos_full_notm}} (COS)

Falls die aktuellen Pakete nicht alle Ihre Anforderungen erfüllen, können Sie jederzeit ein vorhandenes Paket aktualisieren oder ein neues erstellen, um die Regeln an Ihr Unternehmen anzupassen.

### Was ist eine Regel?
{: #ai-rule}

Eine Regel ist die Kombination von Bedingungen und einem einzelnen Ereignis. Sie können einzelne Regeln oder Kombinationen von mehreren Regeln verwenden, um Ergebnisse auszulösen, die in Ihrem {{site.data.keyword.security-advisor_short}}-Dashboard angezeigt werden können.

Beispiel:

```
{
	"comment": "Dormant Rule: Very high risk {{site.data.keyword.appid_short_notm}}  activity... ",
		"dormant": true,
		"conditions": { 	… },
		"event": { … }
	"type": "aggregate"
}
```
{: screen}

Regeln können neben einer Bedingung und einem Ereignis auch die Felder enthalten, die in der folgenden Tabelle aufgeführt sind.

<table>
	<tr>
		<th colspan=2><img src="images/idea.png" alt="Symbol 'Glühbirne'"/> Komponenten einer Regel verstehen</th>
	</tr>
	<tr>
		<td><code>comment</code></td>
		<td>Wird stets ignoriert.</td>
	</tr>
	<tr>
		<td><code>dormant</code></td>
		<td>Ein boolesches Feld, das ignoriert wird, wenn es den Wert 'true' hat. Lautet sein Wert 'false' oder ist kein Wert definiert, so wird die Regel verwendet.</td>
	</tr>
	<tr>
		<td><code>type</code></td>
		<td>Folgende Optionen sind möglich: <code>aggregate</code>, <code>coincident</code> und <code>boolean</code>. Wenn dem Typ nicht der Wert <code>aggregate</code> oder <code>coincident</code> zugewiesen ist, wird er als <code>boolean</code> ausgewertet.</td>
	</tr>
</table>

</br>

### Was ist eine Bedingung?
{: #ai-condition}

Eine Basisbedingung ist ein Baustein, der sich aus drei Komponenten zusammensetzt. Die Blöcke werden mit den Operatoren `any` und `all` gebunden und können verschachtelt sein. Der Operator `all` ist eine funktionale Entsprechung des Operators `and` und `any` ist eine funktionale Entsprechung von `or`.

Beispiel:

```
"conditions": {
	"all": [{
		"any": [{
			"fact": "action",
				"operator": "equal",
				"value": "iam-groups.group.delete"
		},
		{
			"fact": "action",
				"operator": "equal",
				"value": "iam-groups.member.delete"
		}]
	}
}
```
{: screen}

<table>
	<tr>
		<th colspan=2><img src="images/idea.png" alt="Symbol 'Glühbirne'"/> Komponenten einer Regel verstehen</th>
	</tr>
	<tr>
		<td><code>fact</code></td>
		<td>Das CADF-Ereignis von Activity Tracker (CADF - Cloud Auditing Data Federation), das untersucht wird.</td>
	</tr>
	<tr>
		<td><code>operator</code></td>
		<td>Folgende Optionen sind möglich: <code>equal</code>, <code>notEqual</code>, <code>lessThan</code>, <code>greaterThan</code>, <code>in</code> und <code>notIn</code>.</td>
	</tr>
	<tr>
		<td><code>value</code></td>
		<td>Die Art und Weise, wie eine Aktion definiert ist. Der Wert entspricht in der Regel einem API-Aufruf, der für die Interaktion mit dem Service verwendet werden kann.</td>
	</tr>
</table>

</br>

### Was ist ein Ereignis?
{: #ai-event}

An Ereignis setzt sich aus den folgenden zwei Feldern zusammen: `type` und `params.findingType`. Beim ersten Feld handelt es sich um eine eindeutige Kennung für eine Regel, wohingegen das Feld `params.findingType` den Name des Ergebnisses ist, das an den Service ausgegeben wird. Der Name des Ergebnisses ermöglicht die Anzeige des Untersuchungsergebnisses im {{site.data.keyword.security-advisor_short}}-Dashboard.

Beispiel:

```
{
	"conditions": { 	… },
		"event": {
		"type": "IKS high risk API",
			"params": {"findingType": "IKS-high-risk"}
		}
}
```
{: screen}


### Regeltyp: 'aggregate'
{: #rule-aggregate}

Beim Regeltyp 'aggregate' wird die Häufigkeit eines Aktion in einem bestimmten Zeitrahmen gezählt und, wenn diese den definierten Schwellenwert überschreitet, ein Ergebnis ausgelöst. Die Regel wird durch Hinzufügen des Schwellenwerts und des Zeitfensters zur booleschen Bedingung definiert. Damit die Regel definiert werden kann, müssen mehrere Bedingungen erfüllt sein:

* Der Regeltyp muss `aggregate` sein.
* Die Stammbedingung muss die folgenden Fakten enthalten:

	```
	{
			"fact": "occurrences",
			"operator": [equal | greaterThan | greaterThanInclusive],
			"value": [positive Zahl]
	},
	{
	    "fact": "withInLast",
	    "operator": "equal",
	    "value": "X [minutes|hours]",
	}
	```
	{: screen}

	Hierbei müssen die folgenden Erläuterungen beachtet werden:
	* X = positive ganze Zahl ungleich null.
	* Wenn Stunden ausgewählt sind, beträgt der maximal zulässige Wert 24.
	* Wenn Minuten ausgewählt sind, beträgt der maximal zulässige Wert 1440.

#### Beispiel
{: #aggregate-example}

Das folgende Beispiel veranschaulicht eine Regel, die fünf fehlgeschlagene Versuche in einem Zeitraum von 30 Minuten verzeichnet:

```
{
    "conditions": {
        "all": [
            {
                "fact": "action",
                "operator": "equal",
                "value": "iam-identity.user-apikey.login"
            },
            {
                "fact": "reason",
                "operator": "equal",
                "value": 400,
                "path": ".reasonCode"
            },
            {
                "fact": "occurrences",
                "operator": "equal",
                "value": 5
            },
            {
                "fact": "withInLast",
                "operator": "equal",
                "value": "30 minutes",
            }
        ]
    },
    "event": {
        "type": "failed-login-attempts",
        "params": {
            "findingType": "failed-login-attempts",
        }
    },
    "type" : "aggregate"
}
```
{: screen}

### Regeltyp: 'coincident'
{: #rule-coincident}

Bei einem Regeltyp 'coincident' werden Aktionen überwacht, um festzustellen, wie häufig dieselbe Aktion innerhalb eines Zeitfensters stattfindet. Die Regel wird durch Hinzufügen eines Zeitfensters zu einer Gruppe von Bausteinen aus Basisbedingungen definiert. Die Reihenfolge, in der die Aktionen stattfinden, ist in der Regel 'coincident' völlig irrelevant, jedoch gibt es mehrere Bedingungen, die erfüllt sein müssen, damit die Regel definiert werden kann:

* Der Regeltyp muss `coincident` sein.
* Die Stammbedingung muss vom Typ `all` sein.
* Die Stammbedingung muss die folgenden Fakten enthalten:

	```
	{
	    "fact": "actions",
	    "operator": "contains",
	    "value": [Aktionswert]
	},
	{
	    "fact": "withInLast",
	    "operator": "equal",
	    "value": "X [minutes|hours]",
	}
	```
	{: screen}

	Hierbei müssen die folgenden Erläuterungen beachtet werden:
	* Der Wert für `fact` muss im Plural stehen: `actions`, nicht `action`.
	* X = positive ganze Zahl ungleich null.
	* Wenn Stunden ausgewählt sind, beträgt der maximal zulässige Wert 24.
	* Wenn Minuten ausgewählt sind, beträgt der maximal zulässige Wert 1440.


#### Beispiel
{: #coincident-example}

Das folgende Beispiel veranschaulicht eine Regel, die das Zusammenfallen von drei bestimmten Aktionen überwacht, die innerhalb eines Zeitraums von 30 Minuten auftreten müssen:

```
{
    "conditions": {
        "all": [
            {
                "fact": "actions",
                "operator": "contains",
                "value": "iam-identity.user-apikey.login"
            },
            {
                "fact": "actions",
                "operator": "contains",
                "value": "kms.secrets.list"
            },
            {
                "fact": "actions",
                "operator": "contains",
                "value": "kms.secrets.read"
            },
            {
                "fact": "withInLast",
                "operator": "equal",
                "value": "30 minutes",
            }
        ]
    },
    "event": {
        "type": "key-protect-keys-read",
        "params": {
            "findingType": "key-protect-keys-read",
        }
    },
    "type" : "coincident"
}
```
{: screen}

### Regeltyp: 'boolean'
{: #rule-boolean}

Eine Regel vom Typ 'boolean' setzt sich aus einer booleschen Bedingung und einem Ereignis zusammen. Boolesche Regeln werden häufig verwendet, um die risikoreiche API-Nutzung, die API-Nutzung außerhalb des Änderungsmanagementfensters oder die API-Nutzung durch einen Initiator zu überwachen, der nicht in einer Whitelist aufgeführt wird.

Wenn eine Regel nicht als `aggregate` oder `coincident` definiert ist, wird sie als Regel vom Typ `boolean` ausgewertet.

#### Beispiel
{: #boolean-example}

Das folgende Beispiel veranschaulicht eine Regel, die überwacht, ob die Richtlinie außerhalb des Änderungssteuerungsfensters von einem Benutzer gelöscht wird, der nicht auf der Whitelist steht:

```
{
	"conditions": {
		"all": [{
			"any": [{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.group.delete"
			},
			{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.member.delete"
			}]
		},
		{
			"any": [{
				"fact": "event_time",
				"operator": "lessThan",
				"value": "0800"
			},
			{
				"fact": "event_time",
				"operator": "greaterThan",
				"value": "1700"
			}
			]
		},
		{
			"fact": "initiator",
			"path": ".name",
			"operator": "notIn",
			"value": ["example@ibm.com", "example2@ibm.com"]
		}
		]
	},
	"event": {
		"type": "account-delete",
		"params": {
			"findingType": "iam-delete-account-threshold"
		}
	}
```
{: screen}

Möchten Sie mehr über boolesche Regeln erfahren? Werfen Sie einen Blick auf die [Dokumente von CacheControl](https://github.com/CacheControl/json-rules-engine/blob/master/docs/rules.md){: external}.
{: tip}

## Nächste Schritte
{: #activity-next}

Bereit, loszulegen? Dann informieren Sie sich, wie Sie [Activity Insights aktivieren](/docs/services/security-advisor?topic=security-advisor-setup-activity)!
