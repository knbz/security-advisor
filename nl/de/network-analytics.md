---

copyright:
  years: 2018
lastupdated: "2018-11-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}

# Network Analytics
{: #network-analytics}

Mit {{site.data.keyword.security-advisor_long}} können Sie Einblick in möglicherweise gefährliche Netzkommunikation im Zusammenhang mit Ihren {{site.data.keyword.containerlong_notm}}-Clustern gewinnen. Für eine Vorschau dieser Network Analytics-Funktionalität wählen Sie **Vorschau** auf der [Einführungsseite](https://console.bluemix.net/security/advisor/#!/overview) von {{site.data.keyword.security-advisor_short}} aus.{: shortdesc}

Die Network Analytics-Vorschaufunktion besteht aus drei Modulen:

1. **Netzfluss-Erfassungsagent**: Der in Ihrem Cluster installierte Agent erfasst Netzinformationen und sendet sie an das Analyse-Back-End. Weitere Informationen zur Datenerfassung finden Sie unter [Datenerfassung](#data-collection).

2. **Analyse-Back-End**: Das Analyse-Back-End identifiziert verdächtige Clusterkommunikation. Es verwendet von [IBM X-Force![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/security/xforce) bereitgestellte Bedrohungsdaten, um die Netzinformationen zu interpretieren. Wenn verdächtige Kommunikation festgestellt wird, überwacht das Analyse-Back-End diese Kommunikation und sendet Sicherheitsuntersuchungsergebnisse an das {{site.data.keyword.security-advisor_short}}-Dashboard.

3. **{{site.data.keyword.security-advisor_short}}-Dashboard**: Untersuchungsergebnisse und KPIs vom Analyse-Back-End werden auf zwei Karten angezeigt:

   - **Verdächtige Clients (eingehender Datenverkehr)**: Auf dieser Karte werden KPIs und Untersuchungsergebnisse in Bezug auf verdächtige externe Clients, die auf den Cluster zugreifen, angezeigt. Beispielsweise, wenn ein Mitglied eines Botnets mit einer der Cluster-IP-Adressen Kontakt aufnimmt. Weitere Informationen hierzu finden Sie unter [Verdächtige Clients](#suspicious-clients).
   - **Verdächtige Server-IP-Adressen (abgehender Datenverkehr)**: Auf dieser Karte werden KPIs und Untersuchungsergebnisse angezeigt, die [verdächtige Server-IP-Adressen](#suspicious-server-ips) aufweisen, die als Bestandteil des Clusters ausgeführt werden. Beispielsweise, wenn der Cluster Kontakt mit einem Server aufnimmt, der im Verdacht steht, Malware zu verteilen.


## Datenerfassung
{: #data-collection}

Durch die zusätzliche Netzüberwachung kann {{site.data.keyword.security-advisor_short}} zum Schutz Ihres Clusters beitragen. Die als Teil Ihres Clusters implementierte Netzüberwachung wird zur Bereitstellung von Informationen über die Clusterkommunikation verwendet. Zur Aktivierung von Network Analytics werden Datenflussinformationen über die Kommunikation zwischen dem Cluster und externen Entitäten erfasst.
{: shortdesc}

Die folgenden Informationen werden erfasst:

* Die IP-Adresse der kommunizierenden Peers
* Die von ihnen verwendeten Ports
* Die Gruppe der verwendeten Protokolle
* Das Volumen der übertragenen Daten
* Eine Gruppe protokollspezifischer Parameter
* Verschiedene Zeitmarken

**Die tatsächlichen Daten, die ausgetauscht werden, werden nicht erfasst**.

Die Netzüberwachung arbeitet als Bestandteil des Clusters mit anderen Services, z. B. mit {{site.data.keyword.loganalysisshort_notm}}, so dass Sie sich mit der Geschäftslogik befassen können. Sie können die Erkenntnisse der Netzüberwachung in Ihrem {{site.data.keyword.security-advisor_short}}-Dashboard überprüfen.


## Verdächtige Clients (eingehender Datenverkehr)
{: #suspicious-clients}

Das {{site.data.keyword.security-advisor_short}}-Dashboard enthält eine Karte für verdächtige Clients, auf der verschiedene Informationen zu Datenübertragungen zusammengefasst sind, bei denen der Cluster als Server fungiert, der von einem externen Client kontaktiert wird.
{: shortdesc}

Das Untersuchungsergebnis nach der Analyse der Datenübertragung könnte wie folgt aussehen:

- Ein externer Client mit schlechter Reputation, z. B. ein Client, von dem bekannt ist, dass er Malware verteilt oder mit anonymen Services zu tun hat, nimmt Kontakt mit einer der URLs oder IP-Adressen auf, die dem Cluster zugeordnet sind. Beispielsweise eine Suchsoftware, die Kontakt mit einem der geöffneten Cluster-Ports aufnimmt. Eine derartige Kommunikation könnte einen mangelhaften Schutz der dem Cluster zugeordneten URLs und IP-Adressen nahelegen und darüber hinaus eine bevorstehende oder tatsächliche Gefahr.


## Verdächtige Server-IP-Adressen (abgehender Datenverkehr)
{: #suspicious-server-ips}

Das {{site.data.keyword.security-advisor_short}}-Dashboard enthält eine Karte für verdächtige Server-IP-Adressen, auf der verschiedene Informationen zu Datenübertragungen zusammengefasst sind, bei denen der Cluster als Client fungiert, der mit einem externen Server Kontakt aufnimmt.
{: shortdesc}

Das Untersuchungsergebnis nach der Analyse der Datenübertragung könnte wie folgt aussehen:

- Ein Client innerhalb des Clusters, z. B. Workload Deployment, nimmt Kontakt mit einem externen Knoten mit schlechter Reputation auf, z. B. mit einem Botnet. Eine derartige Kommunikation könnte eine Beeinträchtigung der Workload nahelegen und dass sie von Ihrem Sicherheitsteam beachtet werden muss. Das Untersuchungsergebnis ist von der Reputation des kontaktierten externen Knotens, den Kommunikationsmustern und der durch die Ermittlungsinformationen bereitgestellten Zuverlässigkeit abhängig.

- Die Cluster-URL oder eine der IP-Adressen hat eine schlechte Reputation. Überprüfen Sie eine schlechte Reputation, um festzustellen, ob der Cluster gefährdet oder an anderweitigen nicht akzeptablen Aktivitäten beteiligt ist.
