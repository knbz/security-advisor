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

# Integrationen
{: #about}

Mit {{site.data.keyword.security-advisor_long}} können Sie Untersuchungsergebnisse aus Ihren IBM Cloud-Services oder Partnerlösungen integrieren oder eigene angepasste Integrationen erstellen.
{: shortdesc}


## Vorintegrierte Untersuchungsergebnisse
{: #built-in}

Mit {{site.data.keyword.security-advisor_long}} können Sie mithilfe von integrierten Servicefunktionen Einblick in mögliche Probleme erhalten.
{: shortdesc}


{{site.data.keyword.security-advisor_short}} 'out-of-the-box' ist mit Folgendem integriert:

* Certificate Manager für Benachrichtigungen, die sich auf den Ablauf von Zertifikaten und den Lebenszyklus beziehen.
* Vulnerability Advisor für Details zu Sicherheitslücken von Images und Konfigurationsproblemen.

Weitere Informationen finden Sie unter [Vorteile von vorintegrierten Services nutzen](setup.html).

</br>

## Partnerlösungen
{: #partner}

Partnerlösungen bieten die Möglichkeit, die Sicherheit Ihrer IBM Cloud-Bereitstellungen zu verbessern, indem Sie {{site.data.keyword.security-advisor_short}} mit anderen Sicherheitstools verknüpfen, die sich außerhalb von IBM Cloud befinden.
{: shortdesc}

Derzeit ist {{site.data.keyword.security-advisor_short}} Partner folgender Unternehmen:

### NeuVector
{: #neuvector}

[NeuVector](https://neuvector.com/) bietet eine hoch integrierte, automatisierte Sicherheitsplattform für Kubernetes und Red Hat OpenShift, die Ihnen die Überwachung und den Schutz der Kommunikation zwischen Containernetzen ermöglicht. Insbesondere des Netzverkehrs zwischen Ost und West.

Mit NeuVector können Sie Netzbedrohungen und Verstöße erkennen, um Attacken auf Ihre containerbasierten Anwendungen zu verhindern. Durch die Überwachung können Sie Exploits und Aufgliederungen (Breakouts) verhindern, indem Sie Eskalierungen von Rootberechtigungen, Portsuchen, Reverse-Shells und verdächtige Dateisystemaktivitäten in Ihren Containern und Hosts erkennen.

Informationen zur Konfiguration Ihrer NeuVector-Instanz über den Integrationsassistenten finden Sie in [Partnerlösungen](partners.html).

Sind Sie Partner und an der Integration Ihrer Lösung mit {{site.data.keyword.security-advisor_short}} interessiert? Wenden Sie sich an unser Team, indem Sie Matt Ward (wardm@us.ibm.com) kontaktieren.
{: tip}

</br>

## Angepasste Integrationen
{: #custom}

Möglicherweise verfügen Sie bereits über ein Sicherheitstool, auf das sie sich verlassen. Sie können dieses Tool in das {{site.data.keyword.security-advisor_short}}-Dashboard integrieren, sodass Ihre gesamten Sicherheitsinformationen zentral an einer Position vorhanden sind.
{: shortdesc}

**Eigene Tools in die GUI integrieren**

Durch Verwendung der {{site.data.keyword.security-advisor_short}}-GUI können Sie Lesezeichen für Ihre internen und externen Sicherheitstools setzen, sodass Sie über das {{site.data.keyword.security-advisor_short}}-Dashboard als einzigen Zugriffspunkt auf alles zugreifen können. Sie können beispielsweise ein Lesezeichen für PagerDuty setzen.

**Eigene Tools in die API integrieren**

Mithilfe der API für Untersuchungsergebnisse können Sie Untersuchungsergebnisse Ihrer Sicherheitstools in das {{site.data.keyword.security-advisor_short}}-Dashboard integrieren. Es kann sich dabei um beliebige angepasste Tools oder Tools von Partnern handeln, die Sicherheitsalerts oder Untersuchungsergebnisse generieren und die die API-basierte Integration unterstützen.

**Los gehts!**

Informationen zur empfohlenen Praxis für die Verwendung der APIs und dem Verfahren zum Erstellen von Lesezeichen über die GUI finden Sie in [Angepasste Sicherheitstools](/docs/services/security-advisor/custom.html).

</br>


## Network Analytics (Vorschau)
{: #network-analytics}

Mit der Network Analytics-Vorschau können Sie durch Überwachen Ihrer {{site.data.keyword.containerlong_notm}}-Clusterkommunikation Bedrohungen erkennen.
{: shortdesc}

Durch die Überwachung der Clusterkommunikation auf verdächtige Kommunikation von und zu Ihren Clustern können Sie die von IBM X-Force bereitgestellten Bedrohungsdaten nutzen, um potenzielle Probleme zu markieren. Wenn eine verdächtige Kommunikation festgestellt wird, wird ein Untersuchungsergebnis ausgegeben, das Sie prüfen und bewerten können.

Weitere detaillierte Informationen zur Netzanalyse finden Sie in [Network Analytics](network-analytics.html).
{: tip}

</br>
</br>
