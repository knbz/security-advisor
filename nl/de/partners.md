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


# Partner
{: #setup-partner}

IBM Business Partner-Integrationen bieten eine Möglichkeit, kritische Alerts und Untersuchungsergebnisse von Drittanbietern in das {{site.data.keyword.security-advisor_long}}-Dashboard einzubinden. Diese Partner arbeiten mit IBM zusammen, um das Integrationserlebnis für Sie mitzugestalten, zu vereinfachen und zu steuern.
{: shortdesc}

## Vorbereitende Schritte
{: #partner-before}

Stellen Sie vor dem Integrieren von Partnern sicher, dass die folgenden Voraussetzungen erfüllt sind.

* Stellen Sie sicher, dass Sie bei dem Partner, den Sie integrieren möchten, über ein Konto verfügen.
* Stellen Sie sicher, dass Sie über die erforderlichen Administratorberechtigungen verfügen, um die Integrations-URL für den Partnerservice zu generieren.
* Stellen Sie sicher, dass Sie über IAM-Verwaltungszugriff auf {{site.data.keyword.cloud_notm}} und über Managerzugriff auf {{site.data.keyword.security-advisor_short}} verfügen.

## Integrationsassistent
{: #wizard}

Wenn Sie als Administrator über Administratorberechtigungen sowohl für {{site.data.keyword.cloud_notm}}-Konten als auch Partnerkonten verfügen, können Sie für eine schnelle Betriebsbereitschaft den Integrationsassistenten verwenden. Die vier grundlegenden Schritte des Integrationsassistenten sind wie folgt:

* Vertrauen herstellen und Ihre {{site.data.keyword.cloud_notm}}-Konten und Partnerkonten zuordnen
* Erforderliche Informationen wie Berechtigungsnachweise und URLs zwischen den Konten kopieren
* In {{site.data.keyword.security-advisor_short}} Metadaten für Untersuchungsergebnisse von Partnern installieren
* Die Paarbildung validieren, indem ein Untersuchungsergebnis des Partners in {{site.data.keyword.security-advisor_short}} gepostet wird.


## NeuVector integrieren
{: #setup-neuvector}

Mit [NeuVector](https://neuvector.com){: external} können Sie Netzbedrohungen und Verstöße erkennen, um Attacken auf Ihre containerbasierten Anwendungen zu verhindern. Durch die Überwachung können Sie Exploits und Aufgliederungen (Breakouts) verhindern, indem Sie Eskalierungen von Rootberechtigungen, Portsuchen, Reverse-Shells und verdächtige Dateisystemaktivitäten in Ihren Containern und Hosts erkennen.
{: shortdesc}

Zum Integrieren von NeuVector mit {{site.data.keyword.security-advisor_short}} können Sie die folgenden Schritte ausführen:

1. Navigieren Sie zu **Integrationen > Partnerintegrationen** und wählen Sie aus den bereitgestellten Optionen den Eintrag **NeuVector** aus.
2. Verknüpfen Sie Ihre Konten.
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



## Twistlock integrieren
{: #setup-twistlock}

Sie können Risiken verhindern, indem Sie die Bereitstellung von gefährdeten Images in Ihrer Umgebung unterbinden. Mit automatisierter und angepasster Richtliniendurchsetzung bietet [Twistlock](https://www.twistlock.com){: external} in jeder Phase des Anwendungslebenszyklus die volle Kontrolle.
{: shortdesc}

Beim Konfigurieren der Partnerverbindung werden in Ihrem Dashboard zwei Karten erstellt, auf denen die Untersuchungsergebnisse von Twistlock zusammengefasst sind.

Mit **Twistlock-Laufzeit** werden zwei Key Risk Indicators (KRIs) eingeführt:

* Gesamtzahl von Vorfällen und Audits: Ergebnisse, die sich auf Vorfälle oder Audits bei Ihren cloudnativen Workloads beziehen.
* Gesamtzahl von Firewall-Audits: Ergebnisse, die sich auf Probleme im Zusammenhang mit Ihrer Firewall beziehen.

Mit **Twistlock-Sicherheitslücken** wird ein Key Risk Indicator eingeführt:

* Gesamtzahl von Images mit neuen Sicherheitslücken: Ergebnisse, die sich auf Sicherheitslücken beziehen, die in Ihren Container-Image gefunden wurden.

Weitere Informationen zum Unternehmen enthalten die Dokumente zu Twistlock.

### Twistlock konfigurieren
{: #configure-twistlock}

1. Navigieren Sie im {{site.data.keyword.security-advisor_short}}-Dashboard zu **Integrationen > Partnerintegrationen** und wählen Sie aus den bereitgestellten Optionen den Eintrag **Twistlock** aus.

2. Klicken Sie auf **Ja, mein Konto mit {{site.data.keyword.security-advisor_short}} verbinden**.

  Falls Sie nicht bereits ein Konto besitzen, klicken Sie auf **Nein, ich möchte Unterstützung beim Erstellen eines Kontos > Konto erstellen**. Sie können Ihre Daten angeben; das Vertriebsteam von Twistlock setzt sich dann mit Ihnen in Verbindung, damit Sie den Einstieg finden.
  {: note}

3. Geben Sie Ihrer Verbindung einen Namen. Stellen Sie sicher, dass der Name für Ihr Konto eindeutig ist und ausschließlich alphanumerische Zeichen, Leerzeichen und Bindestriche enthält.

4. Optional: Falls Sie nicht bereits über eine Konfigurations-URL verfügen, klicken Sie auf **Registrierungs-URL generieren**, um zur Twistlock-Dokumentation zu wechseln und zu erfahren, wie Sie die URL erstellen. Stellen Sie sicher, dass Sie das Twistlock-Token verwenden, das Sie mit der Lizenz erhalten haben, damit Sie Zugriff auf die Dokumente erhalten.

5. Navigieren Sie im Twistlock-Dashboard zur Registerkarte **Verwalten > Alerts** und klicken Sie auf **Profil hinzufügen**.

6. Wählen Sie in der Dropdown-Liste **Provider** den Eintrag für **{{site.data.keyword.security-advisor_short}}** aus.

7. Klicken Sie auf **Kopieren**, um die angegebene Konfigurations-URL zu verwenden.

8. Kehren Sie zurück zum {{site.data.keyword.security-advisor_short}}-Dashboard und fügen Sie dort die URL in das Feld **Konfigurations-URL für Twistlock** ein.

9. Klicken Sie auf **Partner verbinden**.

### Verbindung überprüfen
{: #twistlock-verify}

1. Überprüfen Sie im {{site.data.keyword.security-advisor_short}}-Dashboard, ob die Twistlock-Karten wie erwartet angezeigt werden.

2. Aktualisieren Sie im Twistlock-Dashboard die Anzeige der Registerkarte **Alerts**. Es wird die {{site.data.keyword.security-advisor_short}}-Verbindung angezeigt. Öffnen Sie die Verbindung.

3. Vergewissern Sie sich, dass die Alerttypen, über die Sie Benachrichtigungen erhalten möchten, ausgewählt sind, und stellen Sie dann durch Klicken auf **Verifizieren** sicher, dass die Verbindung abgeschlossen ist.
