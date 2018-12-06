---

copyright:
  years: 2018
lastupdated: "2018-11-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Informationen zu {{site.data.keyword.security-advisor_short}}
{: #about}

Mit {{site.data.keyword.security-advisor_long}} können Sicherheitsadministratoren Sicherheitsprobleme in ihren Cloudanwendungen und Cloud-Workloads suchen, priorisieren und verwalten.
{:shortdesc}

{{site.data.keyword.security-advisor_short}} zentralisiert die Insights-Services, wie z. B. Vulnerability Advisor, und {{site.data.keyword.cloudcerts_short}} in {{site.data.keyword.Bluemix_notm}}. Sie können Sicherheitsereignisse verwalten und Analysen anwenden, um Untersuchungsergebnisse zu erstellen, die Ihre Sicherheitsmanagementprozesse in {{site.data.keyword.Bluemix_notm}} vereinheitlichen und verbessern.

{{site.data.keyword.security-advisor_short}} bietet auch eine Testfunktion für die Netzanalyse, die in Ihrem {{site.data.keyword.containerlong_notm}}-Cluster ausgeführt wird und Netzflussdaten erfasst und die die Kommunikation mit verdächtigen Clients und Server-IP-Adressen erkennen kann.

## Wichtige Begriffe
{: #concepts}

Hier finden Sie Informationen zu verschiedenen Begriffen, die bei der Arbeit mit {{site.data.keyword.security-advisor_short}} auftauchen können.
{: shortdesc}

<dl>
  <dt>Untersuchungsergebnis</dt>
    <dd>Ein Untersuchungsergebnis ist ein prioritätsbezogenes Sicherheitsproblem, das bei der Verarbeitung von unformatierten Ereignissen erstellt wird. Untersuchungsergebnisse bestehen aus den einzelnen Schlüsselinformationen, die erforderlich sind, um die Frage nach dem Wer, Was, Wann und Wo des Problems zu beantworten. Als Sicherheitsadministrator können Sie mithilfe von {{site.data.keyword.security-advisor_short}}-Untersuchungsergebnissen erkannte Situationen priorisieren und darauf reagieren.</dd>
  <dt>Key Performance Indicator (KPI)</dt>
    <dd>Ein Key Performance Indicator (wesentlicher Leistungsindikator) wird ausgelöst, wenn der Wert eines Untersuchungsergebnisses außerhalb des vertretbaren Leistungsbereichs für bestimmte Sicherheitsmaßnahmen für Services und Workloads liegt.</dd>
  <dt>Anmerkung</dt>
    <dd>Sie können Anmerkungen erstellen, um die Untersuchungsergebnisse zu kategorisieren, die während der Analyse angezeigt werden. Eine Anmerkung kann in verschiedenen Providern mehrfach vorkommen.</dd>
  <dt>Vorkommen</dt>
    <dd>In einem Vorkommen werden providerspezifische Details einer Anmerkung beschrieben. Das Vorkommen enthält die Details zur Sicherheitslücke, die Korrekturschritte und andere allgemeine Informationen.</dd>
  <dt>Service-CRN</dt>
    <dd>Der Service-CRN gibt den {{site.data.keyword.Bluemix_notm}}-Service an, der an dem Untersuchungsergebnis beteiligt ist.</br>
      <table>
        <tr>
          <th>Typ des Untersuchungsergebnisses</th>
          <th>CRN</th>
        </tr>
        <tr>
          <td>{{site.data.keyword.cloudcerts_short}}</td>
          <td>Der CRN der Serviceinstanz.</td>
        </tr>
        <tr>
          <td>Network Analytics</td>
          <td>Der CRN des Kubernetes-Clusters.</td>
        </tr>
        <tr>
          <td>Vulnerability Advisor</td>
          <td>Der CRN des Containers.</td>
        </tr>
      </table></dd>
    <dt>Ressourcen-CRN</dt>
      <dd>Der Ressourcen-CRN gibt die Ressource an, die an dem Untersuchungsergebnis beteiligt ist.</dd>
</dl>
