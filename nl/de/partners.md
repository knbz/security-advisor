---

copyright:
  years: 2018
lastupdated: "2018-12-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Partnerlösungen
{: #setup-partner}

Partnerlösungen bieten die Möglichkeit, die Sicherheit zu verbessern, indem eine Verknüpfung zwischen {{site.data.keyword.security-advisor_long}} und anderen Sicherheitstools erstellt wird.
{: shortdesc}

## Integrationsassistent
{: #wizard}

Wenn Sie als Administrator über Administratorberechtigungen für sowohl IBM Cloud-Konten als auch Partnerkonten verfügen, können Sie für eine schnelle Betriebsbereitschaft den Integrationsassistenten verwenden. Die vier grundlegenden Schritte des Integrationsassistenten sind wie folgt:

* Vertrauen herstellen und Ihre IBM Cloud-Konten und Partnerkonten zuordnen
* Erforderliche Informationen wie Berechtigungsnachweise und URLs zwischen den Konten kopieren
* In {{site.data.keyword.security-advisor_short}} Metadaten für Untersuchungsergebnisse von Partnern installieren
* Die Paarbildung prüfen, indem ein Untersuchungsergebnis des Partners in {{site.data.keyword.security-advisor_short}} gepostet wird.

</br>

## NeuVector integrieren
{: #neuvector}

**Vorbereitende Schritte**

* Stellen Sie sicher, dass Sie bei dem Partner, den Sie integrieren möchten, über ein Konto verfügen.
* Stellen Sie sicher, dass Sie über die erforderlichen Administratorberechtigungen verfügen, um die Integrations-URL für den Partnerservice zu generieren.
* Stellen Sie sicher, dass Sie über IAM-Verwaltungszugriff auf IBM Cloud und über Managerzugriff auf {{site.data.keyword.security-advisor_short}} verfügen.

**Integration**

1. Verknüpfen Sie Ihre Konten.
  1. Geben Sie einen Namen für Ihre Verbindung an.
  2. Geben Sie die URL für das NeuVector-Dashboard an.
  3. Geben Sie die URL für die NeuVector-Konfiguration an.
    1. Melden Sie sich bei NeuVector an und navigieren Sie zu den Einstellungen.
    2. Klicken Sie auf **URL generieren**.
    3. Kopieren Sie die URL und fügen Sie sie in den {{site.data.keyword.security-advisor_short}}-Integrationsassistenten ein.
  4. Klicken Sie auf **Weiter**.
3. Überprüfen Sie das Erteilen der Genehmigung, sodass der Service eine Service-ID und einen API-Schlüssel erstellen kann, und stellen Sie die Verbindung zu diesem Service her, indem Sie auf **Weiter** klicken.
4. Klicken Sie auf **Fertig**.
5. Rufen Sie Ihr Service-Dashboard auf, um ein Untersuchungsergebnis, das von NeuVector an {{site.data.keyword.containershort_notm}} gesendet wurde, testweise zu überprüfen.
