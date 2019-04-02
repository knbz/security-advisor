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



# Eventi di {{site.data.keyword.cloudaccesstrailshort}}
{: #at_events}

Puoi visualizzare, gestire e controllare le attività avviate dall'utente effettuate nella tua istanza del servizio {{site.data.keyword.security-advisor_long}} utilizzando il servizio {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

Per ulteriori informazioni su come funziona il servizio, consulta la [documentazione {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker/reference?topic=cloud-activity-tracker-getting-started-with-cla#getting-started-with-cla).


## Dove visualizzare gli eventi
{: #monitor}

Gli eventi sono disponibili nel **dominio dell'account** {{site.data.keyword.cloudaccesstrailshort}} disponibile nella regione {{site.data.keyword.Bluemix_notm}} in cui vengono generati gli eventi.

1. Accedi al tuo account {{site.data.keyword.Bluemix_notm}}.
2. Dal catalogo, esegui il provisioning di un'istanza del servizio {{site.data.keyword.cloudaccesstrailshort}} nello stesso account della tua istanza di {{site.data.keyword.security-advisor_short}}.
3. Sulla scheda **Gestisci** del dashboard {{site.data.keyword.cloudaccesstrailshort}}, fai clic su **Visualizza in Kibana**.
4. Imposta l'intervallo di tempo per il quale desideri visualizzare i log. Il valore predefinito è 15 minuti.
5. Nell'elenco **Campi disponibili**, fai clic su **type**. Fai clic sull'icona della lente di ingrandimento del **Programma di traccia dell'attività** per limitare i log a quelli tracciati dal servizio.
6. Puoi utilizzare gli altri campi disponibili per restringere la tua ricerca.

Per utenti diversi dal proprietario dell'account, per visualizzare i log, devi utilizzare il piano premium. Per consentire ad altri utenti di visualizzare gli eventi, consulta [Concessione di autorizzazioni per visualizzare gli eventi di account](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions).
{: tip}

## Elenco degli eventi
{: #events}

Consulta la seguente tabella per un elenco di eventi inviati a {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

<table>
  <tr>
    <th>Azione</th>
    <th>Descrizione</th>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>Visualizza una nota o le note create precedentemente.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>Crea una nota.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>Elimina una nota.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>Aggiorna una nota.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>Visualizza una o più ricorrenze.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>Crea una ricorrenza.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>Elimina una ricorrenza.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>Aggiorna una ricorrenza.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>Leggi le soluzioni personalizzate che sono state aggiunte al servizio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>Aggiungi una soluzione personalizzata al servizio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>Aggiorna una soluzione personalizzata esistente all'interno del servizio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>Elimina una soluzione personalizzata dal servizio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>Visualizza le soluzioni partner che hai aggiunto alla tua istanza del servizio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>Aggiungi una soluzione partner alla tua istanza del servizio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>Aggiorna una soluzione partner nella tua istanza del servizio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>Elimina una soluzione partner dalla tua istanza del servizio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.enable</code></td>
    <td>Abilita la funzione Network Insights fornita da {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.disable</code></td>
    <td>Disabilita la funzione Network Insights fornita da {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.enable</code></td>
    <td>Abilita la funzione Activity Insights fornita da {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.disable</code></td>
    <td>Disabilita la funzione Activity Insights fornita da {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code>security-advisor.insights-cos.create</code></td>
    <td>Crea un'istanza Cloud Object Storage tramite {{site.data.keyword.security-advisor_short}} per Network e Activity Insights.</td>
  </tr>
</table>
