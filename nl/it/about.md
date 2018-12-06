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

# Informazioni su {{site.data.keyword.security-advisor_short}}
{: #about}

Con la sicurezza {{site.data.keyword.security-advisor_long}}, gli amministratori possono trovare, assegnare la priorità e gestire i problemi di sicurezza nei loro carichi di lavoro e applicazioni cloud.
{:shortdesc}

{{site.data.keyword.security-advisor_short}} centralizza i servizi di informazioni approfondite come ad esempio il Controllo vulnerabilità e {{site.data.keyword.cloudcerts_short}} in {{site.data.keyword.Bluemix_notm}}. Puoi gestire gli eventi di sicurezza e applicare le analisi per creare delle ricerche che unificano e migliorano i tuoi processi di gestione della sicurezza su {{site.data.keyword.Bluemix_notm}}.

{{site.data.keyword.security-advisor_short}} offre anche una funzionalità di analisi di rete di prova che viene eseguita sul tuo cluster {{site.data.keyword.containerlong_notm}} per raccogliere i dati del flusso di rete che possono rilevare la comunicazione con i client e gli IP server sospetti.

## Concetti chiave
{: #concepts}

Trova ulteriori informazioni sui diversi concetti che puoi utilizzare mentre lavori con {{site.data.keyword.security-advisor_short}}.
{: shortdesc}

<dl>
  <dt>Ricerca</dt>
    <dd>Una ricerca è un problema di sicurezza prioritario creato quando vengono elaborati gli eventi non elaborati. Le ricerche sono composte da pezzi di informazioni chiave necessari per identificare chi e cosa ha causato il problema e quando e dove si è verificato. Come amministratore della sicurezza, puoi utilizzare le ricerche {{site.data.keyword.security-advisor_short}} per assegnare la priorità e reagire alle situazioni rilevate.</dd>
  <dt>KPI (Key Performance Indicator)</dt>
    <dd>Viene attivato un KPI (Key Performance Indicator) quando il valore di una ricerca è al di fuori dei limiti dalla gamma di prestazioni accettabili dei controlli di sicurezza specifici sui servizi e sui carichi di lavoro.</dd>
  <dt>Nota</dt>
    <dd>Puoi creare delle note per categorizzare le ricerche che hai trovato durante l'analisi. Una nota può verificarsi più volte in diversi provider.</dd>
  <dt>Ricorrenza</dt>
    <dd>Una ricorrenza descrive i dettagli specifici del provider di una nota. La ricorrenza contiene i dettagli della vulnerabilità, le correzioni e altre informazioni generali.</dd>
  <dt>CRN del servizio</dt>
    <dd>Il CRN del servizio identifica il servizio {{site.data.keyword.Bluemix_notm}} coinvolto nella ricerca.</br>
      <table>
        <tr>
          <th>Tipo di ricerca</th>
          <th>CRN</th>
        </tr>
        <tr>
          <td>{{site.data.keyword.cloudcerts_short}}</td>
          <td>Il CRN dell'istanza del servizio. </td>
        </tr>
        <tr>
          <td>Analisi di rete </td>
          <td>Il CRN del cluster Kubernetes.</td>
        </tr>
        <tr>
          <td>Controllo vulnerabilità</td>
          <td>Il CRN del contenitore.</td>
        </tr>
      </table></dd>
    <dt>CRN della risorsa</dt>
      <dd>Il CRN della risorsa identifica la risorsa specifica coinvolta nella ricerca.</dd>
</dl>
