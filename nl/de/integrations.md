---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-14"

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


# Integrationen
{: #integrations}

Mit {{site.data.keyword.security-advisor_long}} können Sie andere integrierte Insight-Features oder Partnerlösungen integrieren oder eigene angepasste Integrationen erstellen.
{: shortdesc}


## Vorintegrierte Untersuchungsergebnisse
{: #integrate-built-in}

Mit {{site.data.keyword.security-advisor_long}} können Sie mithilfe von integrierten Servicefunktionen Einblick in mögliche Probleme erhalten.
{: shortdesc}


Ohne Vorbereitungs- oder Anpassungsaufwand ist {{site.data.keyword.security-advisor_short}} mit Folgendem integriert:

* Certificate Manager für Benachrichtigungen, die sich auf den Ablauf von Zertifikaten und den Lebenszyklus beziehen.
* Vulnerability Advisor für Details zu Sicherheitslücken von Images und Konfigurationsproblemen.

Weitere Informationen finden Sie unter [Vorteile von vorintegrierten Services nutzen](/docs/services/security-advisor?topic=security-advisor-setup-services).

</br>

## Partnerintegrationen
{: #integrate-partner}

Partnerlösungen sind eine Möglichkeit, die Sicherheit Ihrer {{site.data.keyword.cloud_notm}}-Bereitstellungen funktional zu erweitern, indem Sie zur Sicherstellung eines nahtlosen Benutzererlebnisses {{site.data.keyword.security-advisor_short}} mit anderen Sicherheitstools verbinden, die mit IBM zusammenarbeiten.
{: shortdesc}

Zu den aktuellen {{site.data.keyword.security-advisor_short}}-Partnern zählen unter anderem NeuVector und Twistlock.

Sind Sie Partner und an der Integration Ihrer Lösung mit {{site.data.keyword.security-advisor_short}} interessiert? Wenden Sie sich an unser Team, indem Sie Matt Ward (wardm@us.ibm.com) kontaktieren.
{: tip}

### NeuVector
{: #integrate-neuvector}

[NeuVector](https://neuvector.com/) bietet eine hoch integrierte, automatisierte Sicherheitsplattform für Kubernetes und Red Hat OpenShift, die Ihnen die Überwachung und den Schutz der Kommunikation zwischen Containernetzen ermöglicht. Insbesondere des Netzverkehrs zwischen Ost und West.

Mit NeuVector können Sie Netzbedrohungen und Verstöße erkennen, um Attacken auf Ihre containerbasierten Anwendungen zu verhindern. Durch die Überwachung können Sie Exploits und Aufgliederungen (Breakouts) verhindern, indem Sie Eskalierungen von Rootberechtigungen, Portsuchen, Reverse-Shells und verdächtige Dateisystemaktivitäten in Ihren Containern und Hosts erkennen.

Hilfe bei der Einrichtung Ihrer NeuVector-Instanz finden Sie unter [Partnerlösungen](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-neuvector).


### Twistlock
{: #integrate-twistlock}

Twistlock ist in einzigartiger Weise in der Lage, Risiken im Verlauf des gesamten SDLC zu vermeiden, indem es die Bereitstellung von gefährdeten Images in Ihrer Umgebung verhindert. Die automatisierte und angepasste Richtliniendurchsetzung bietet volle Kontrolle in jeder Phase des Anwendungslebenszyklus. Der Twistlock Intelligence Stream verwendet über 30 vorgelagerte Projekte, kommerzielle Quellen sowie die proprietäre Forschung von Twistlock Labs als Quelle und aggregiert direkt aus ihnen Informationen zu Schwachstellen und Sicherheitslücken.

Der Fokus liegt auf der Verfügbarkeit der präzisesten Daten, um alle Schichten Ihres Stacks abzudecken, damit Sie eine korrekte Sichtbarkeit erhalten und die niedrigste Rate an 'falsch-positiv'-Situationen haben. Twistlock kombiniert diese Daten mit dem Wissen um Ihre tatsächlichen Bereitstellungen, z.B. welche Container gegenüber dem Internet zugänglich sind, welche mit umfangreichen Rechten ausgeführt werden und welche anderen Sicherheitsmaßnahmen getroffen wurden, so dass Sie stets im Blick haben, welche Schwachstellen in Ihrer speziellen Umgebung am kritischsten sind.


Hilfe bei der Einrichtung Ihrer Twistlock-Instanz finden Sie unter [Partnerlösungen](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-twistlock).
</br>


## Angepasste Integrationen
{: #integrate-custom}

Möglicherweise verfügen Sie bereits über ein Sicherheitstool, auf das Sie sich verlassen. Sie können dieses Tool in das {{site.data.keyword.security-advisor_short}}-Dashboard integrieren, sodass Ihre gesamten Sicherheitsinformationen zentral an einer Position vorhanden sind.
{: shortdesc}

### Direktverbindung erstellen
{: #create-direct-connect}

Durch Verwendung der {{site.data.keyword.security-advisor_short}}-GUI können Sie Lesezeichen für Ihre internen und externen Sicherheitstools setzen, sodass Sie über das {{site.data.keyword.security-advisor_short}}-Dashboard als einzigen Zugriffspunkt auf alles zugreifen können. Sie können beispielsweise ein Lesezeichen für PagerDuty setzen.

### Eigene Tools in die API integrieren
{: #integrate-tools-api}

Mithilfe der API für Untersuchungsergebnisse können Sie Untersuchungsergebnisse Ihrer Sicherheitstools in das {{site.data.keyword.security-advisor_short}}-Dashboard integrieren. Es kann sich dabei um beliebige angepasste Tools oder Tools von Partnern handeln, die Sicherheitsalerts oder Untersuchungsergebnisse generieren und die die API-basierte Integration unterstützen.

## Integrierte Insights-Features
{: #integrate-insights}

Mit den integrierten Insights-Features können Sie potenzielle Probleme erkennen, indem Sie Ihre Cluster- und Kontoprotokolle kontinuierlich überwachen lassen. Durch die Überwachung des Datenaustauschs im Netz und der Benutzeraktivität können Sie sicherstellen, dass Ihre {{site.data.keyword.cloud_notm}}-Ressourcen weiterhin geschützt bleiben.
{: shortdesc}

**Network Insights (Betaversion)**

Mit Network Insights (Betaversion) können Sie sowohl die eingehende als auch die abgehende Netzkommunikation zwischen Ihrem Kubernetes-Cluster und externen Entitäten überwachen und analysieren. Durch den Einsatz der integrierten Funktion für Bedrohungsermittlung und Anomalieerkennung kann der Service Reconnaissance-Angriffe und potenziell beeinträchtigte Assets erkennen. Weitere Informationen finden Sie unter [Network Insights](/docs/services/security-advisor?topic=security-advisor-network).

**Activity Insights (Vorschau)**

Mit Activity Insights (Vorschau), können Sie Ihre {{site.data.keyword.cloud_notm}} Activity Tracker-Protokolle fortlaufend überwachen, um nicht autorisierte oder verdächtige Aktivitäten zu identifizieren, die von Benutzern oder Apps unter Verwendung von Regelpaketen ausgeführt werden. Sie können die vom Service bereitgestellten Regelpakete verwenden, die auf bewährten Verfahren (Best Practice) für Sicherheit basieren, oder Sie können die Regeln an Ihre jeweiligen Bedürfnisse anpassen. Weitere Informationen finden Sie unter [Activity Insights](/docs/services/security-advisor?topic=security-advisor-activity).
