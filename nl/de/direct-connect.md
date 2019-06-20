---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

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


# Direktverbindungen
{: #setup-custom-gui}

Mit {{site.data.keyword.security-advisor_short}} können Sie Ihre vorhandenen angepassten Sicherheitstools integrieren, unabhängig davon, ob es sich um Open-Source-Services, Services für die angepassten Entwicklung oder um Services anderer Anbieter handelt. Durch Hinzufügen der URL zur Liste Ihrer Integrationen können Sie eine Direktverbindung von {{site.data.keyword.security-advisor_short}} zu dem anderen Tool erstellen. Durch diese Verbindung können Sie alle von Ihnen verwendeten Tools ohne großen Aufwand überwachen.
{: shortdesc}


## Vorbereitende Schritte
{: #custom-before-gui}

Damit Sie die Integration hinzufügen können, müssen Sie bei dem Partner, den Sie integrieren möchten, über ein Konto verfügen.

{{site.data.keyword.security-advisor_short}} definiert Berechtigungsnachweise, die sich auf den Partnerservice beziehen, nicht als persistent. Unternehmensbenutzer müssen sich mithilfe von SAML sowohl bei der {{site.data.keyword.cloud_notm}}-Instanz als auch bei dem Geschäftspartner authentifizieren.
{: note}

## Verbindung konfigurieren
{: #custom-configure-connection}

1. Melden Sie sich bei Ihrem Sicherheitstool an und rufen Sie Ihre eindeutige URL ab.

2. Melden Sie sich über die Konsole bei {{site.data.keyword.cloud_notm}} an.

3. Klicken Sie auf **Angepasste Integrationen > Direktverbindung**. Eine Anzeige wird geöffnet.

  1. Geben Sie Ihrer Lösung einen Namen. Sie dürfen nur alphanumerische Zeichen, Leerzeichen und Bindestriche (-) im Namen verwenden.

  2. Geben Sie die URL für die Lösung im folgenden Format ein: `www.<website>.<domain>`.

  3. Laden Sie ein Symbol oder eine Grafik hoch, um das Tool darzustellen.

  4. Klicken Sie auf **Verbindung herstellen**, um die Konfiguration abzuschließen. {{site.data.keyword.security-advisor_short}} erstellt die für die Integration erforderlichen Artefakte, z. B. die Service-ID, den API-Schlüssel, die Konto-ID und die Metadaten. Die Rolle des Schreibberechtigten (`writer`) wird zugewiesen.
