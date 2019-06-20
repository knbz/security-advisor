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


# Network Insights (Betaversion)
{: #network}

Mit {{site.data.keyword.security-advisor_long}} können Sie Erkenntnisse auf der Grundlage von Bedrohungsinformationen, Verhaltensmustern und maschinellem Lernen für potenziell gefährdete Container gewinnen, deren Ausführung in Ihren {{site.data.keyword.containerlong_notm}}-Clustern erfolgt.
{: shortdesc}


## Funktionsweise
{: #network-how-it-works}

Das Network Insights-Feature ist ein Add-on für den {{site.data.keyword.security-advisor_short}}-Service. Wenn die Funktion aktiviert und konfiguriert ist, wird sowohl die eingehende als auch die abgehende Netzkommunikation zwischen Ihrem Cluster und externen Einheiten erfasst und fortlaufend überwacht sowie analysiert.

Die folgende Abbildung enthält eine Darstellung des Informationsflusses.

![Network Insights-Flussdiagramm](images/network-insights-flow.png)

1. Als Kontoadministrator können Sie Network Insights in jedem Cluster installieren, den Sie überwachen möchten.
2. Die Netzflussprotokolle werden an ein Cloud Object Storage-Bucket weitergeleitet, in dem sie gespeichert werden, bis Sie beschließen, sie zu löschen. Wenn Sie die Benutzerschnittstelle (GUI) von {{site.data.keyword.security-advisor_short}} zum Erstellen des Buckets verwenden, werden Service-zu-Service-IAM-Rollen zugewiesen, sodass der Service die Protokolle anzeigen kann.
3. Wenn Network Insights aktiviert ist, werden die Rohdaten in Ihrem COS-Bucket auf verdächtiges Verhalten analysiert.
4. Wenn ein mögliches Sicherheitsproblem markiert wird, wird das Ergebnis an die Datenbank für Untersuchungsergebnisse weitergeleitet.
5. Untersuchungsergebnisse werden in Ihrem Service-Dashboard auf drei Karten angezeigt:
  * Verdächtiger eingehender Datenverkehr
  * Verdächtiger abgehender Datenverkehr
  * Anomalien im Datenverkehr


## Daten erfassen
{: #network-data}

{{site.data.keyword.security-advisor_short}} stellt einen Collector zur Verfügung, der zum Erfassen von Informationen zum Netzfluss zwischen einem Cluster und externen Entitäten verwendet wird.
{: shortdesc}

Die erfassten Rohdaten werden in einem Cloud Object Storage-Bucket gespeichert, für den Sie die Speicherdauer bestimmen können. Sie sind der Eigner der erfassten Daten und Ihnen obliegt die Kontrolle über sie, was bedeutet, dass Sie für ihre Speicherung, Sicherung und Löschung verantwortlich sind. {{site.data.keyword.security-advisor_short}} bewahrt Untersuchungsergebnisse 90 Tage lang auf. Während dieser Zeit werden die Ergebnisse im Service-Dashboard auf den Network Insight-Karten dargestellt. Obwohl das Ergebnis also nach 90 Tagen nicht mehr in Ihrem Dashboard angezeigt wird, ist es durchaus möglich, dass sich die Rohdaten noch im Speicher befinden.

Die folgenden Informationen werden erfasst:

* Die IP-Adresse der kommunizierenden Peers
* Die von ihnen verwendeten Ports
* Die Gruppe der verwendeten Protokolle
* Das Volumen der übertragenen Daten
* Eine Gruppe protokollspezifischer Parameter
* Verschiedene Zeitmarken

Die tatsächlich ausgetauschten Daten werden nicht erfasst.
{: tip}

Aus dem Blickwinkel der Sicherheit ist es sinnvoll, die erfassten Daten zu bereinigen, wenn gesetzliche oder betriebliche Voraussetzungen das Löschen zulassen. Weitere Informationen finden Sie in [Objekte löschen](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion).


## Netz: Verdächtiger eingehender Datenverkehr
{: #network-suspicious-inbound}

{{site.data.keyword.security-advisor_short}} informiert Sie im Service-Dashboard auf der Karte **Verdächtiger eingehender Datenverkehr** über Versuche externer Clients, Cluster zu überprüfen und zu gefährden.
{: shortdesc}


Die Verhaltensmuster von Clients, die IBM X-Force als verteilende Malware klassifiziert, deren Verwendung als Scanner, als Teil eines Botnets, für das Mining von Kryptowährung(en) oder für Anonymisierungsservices erfolgt, werden fortlaufend überwacht. Wenn diese Art von Client Kontakt zu einem überwachten Cluster aufnimmt und ein alarmierendes Verhalten aufweist, gibt Network Insights ein Untersuchungsergebnis aus.


Auf dieser Karte werden zwei Key Risk Indicators (KRIs) eingeführt:

* Reconnaissance durch verdächtige Clients: Umfasst Ergebnisse, die sich auf Clients beziehen, die auf den Cluster zugreifen.
* Von verdächtigen Clients gesendete ungewöhnlich umfangreiche Nutzdaten: Umfasst Ergebnisse, die sich auf Datenvolumen beziehen, die zwischen Clients und dem Cluster gesendet werden. Als ungewöhnliche Nutzlast gilt jegliches Verhalten, das untypisch für Ihren Cluster ist.


Die Ergebnisse können verdächtige Clients einbeziehen, auf die Folgendes zutrifft:

* Senden von ungewöhnlich großen Datenvolumen in den Cluster.
* Durchführen einer ungewöhnlichen großen Anzahl von Anforderungen an einen Cluster-Service.
* Scheinbares Abzielen auf den Cluster, wie die Anzahl von Versuchen zeigt, die unternommen werden, um den Cluster zu überprüfen.



## Netz: Verdächtiger abgehender Datenverkehr
{: #network-suspicious-outbound}

{{site.data.keyword.security-advisor_short}} informiert Sie im Service-Dashboard auf der Karte **Verdächtiger abgehender Datenverkehr** über alle potenziell beeinträchtigten Container, die in Ihren {{site.data.keyword.containershort_notm}}-Clustern ausgeführt werden.
{: shortdesc}

Der Service überwacht fortlaufend die Verhaltensmuster von Clients, die IBM X-Force als verteilende Malware klassifiziert, deren Verwendung als Scanner, als Teil eines Botnets, für das Mining von Kryptowährung(en) oder für Anonymisierungsservices erfolgt. Nachdem sich ein Container auf einem überwachten Cluster den verdächtigen Peers nähert und ein alarmierendes Verhalten aufweist, gibt Network Insights ein Untersuchungsergebnis aus.

Auf dieser Karte werden zwei Key Risk Indicators (KRIs) eingeführt:

* Abgehende Kontaktaufnahme zu verdächtigen Servern: Ergebnisse, die sich auf einen Cluster beziehen, auf den die Server zugreifen.
* Mit verdächtigen Servern ausgetauschte ungewöhnlich umfangreiche Nutzdaten: Ergebnisse, die sich auf Datenvolumen beziehen, die zwischen dem Cluster und Servern gesendet werden.


Die Ergebnisse können Container einbeziehen, auf die Folgendes zutrifft:

* Senden ungewöhnlich großer Datenvolumen an einen verdächtigen Server.
* Extrahieren eines großen Datenvolumens aus einem verdächtigen Server.
* Durchführen einer großen Anzahl von Anforderungen an einen verdächtigen Server.


## Netz: Anomalien im Datenverkehr (experimentell)
{: #network-anomalies}

In diesem experimentellen Feature überwacht {{site.data.keyword.security-advisor_short}} Ihr Netz, um Verhaltensmuster zu erlernen. Nach der Bildung von Mustern wird jegliches Verhalten, das ungewöhnlich (anormal) erscheint, entsprechend markiert und im Service-Dashboard auf der Karte **Anomalien im Datenverkehr** zusammengefasst.
{: shortdesc}

Ein Modell des gewöhnlichen (normalen) Containerverhaltens wird durch die Überwachung der Verhaltensmuster zwischen einem Container und seinen Peers erstellt. Wenn das Modell erfasst ist, wird es auf bestimmte Änderungen hin überwacht. Falls eine alarmierende Änderung festgestellt wird, gibt Network Insights ein Untersuchungsergebnis aus.

Auf dieser Karte werden zwei Key Risk Indicators (KRIs) eingeführt:

* Stärkere Reconnaissance- oder Datenaustauschaktivität als gewöhnlich: Ergebnisse, die sich auf anormale Interaktionen beziehen, die zwischen dem Cluster und jeglichen externen Peers festgestellt wurden.
* Abgehende Kontaktaufnahme zu neuem Server: Ergebnisse, die sich auf neu erkannte Server beziehen, zu denen der Cluster Kontakt aufnimmt.

Ein Ergebnis kann Folgendes einbeziehen:  

* Container, die Kontakt zu Servern aufnehmen, die noch nie zuvor kontaktiert wurden.
* Container, die deutlich mehr Daten als gewöhnlich an bestimmte Peers senden oder von bestimmten Peers verarbeiten.
* Der Grad der Überprüfung eines bestimmten Containers hat beträchtlich zugenommen.

Nach der Entwicklung des Modells werden Abweichungen vom erlernten Modell erkannt und wenn eine alarmierende Änderung auftritt, veröffentlicht Network Insights ein Untersuchungsergebnis im Dashboard von Security Advisor. Die Ergebnisse werden auf der Karte "Network: Anomalies in Traffic" zusammengefasst. Auf dieser Karte werden zwei Key Risk Indicators (KRIs) eingeführt. Der KRI "Stärkere Reconnaissance- oder Datenaustauschaktivität als gewöhnlich" erfasst Ergebnisse, die sich auf anormale Interaktionen beziehen, die zwischen dem Cluster und externen Peers festgestellt wurden. Der KRI "Abgehende Kontaktaufnahme zu neuem Server" erfasst Ergebnisse, die sich auf neu erkannte Server beziehen, zu denen der Cluster Kontakt aufnimmt.  

## Nächste Schritte
{: #network-next}

Bereit, loszulegen? Dann informieren Sie sich, wie Sie [Network Insights aktivieren](/docs/services/security-advisor?topic=security-advisor-setup-network)!
