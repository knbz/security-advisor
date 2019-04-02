---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-13"

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


# Personalizzato
{: #setup_custom}

{{site.data.keyword.security-advisor_short}} ti consente di integrare i tuoi strumenti di sicurezza personalizzati esistenti, sia che siano servizi open source, sviluppati personalizzati o di terze parti. Puoi creare una connessione diretta da {{site.data.keyword.security-advisor_short}} a un altro strumento aggiungendo l'URL al tuo elenco di integrazioni. Puoi inoltre integrare le ricerche di terze parti per utilizzare gli eventi di sicurezza critici direttamente in una nuova scheda nel dashboard utilizzando le API per configurare le API e i KRI (key risk indicator).
{: shortdesc}


## Aggiunta di una connessione diretta
{: #setup-custom-gui}

Puoi facilmente tenere traccia dei tuoi strumenti di sicurezza utilizzando il dashboard {{site.data.keyword.security-advisor_short}}.
{: shortdesc}

### Prima di cominciare
{: #custom-before-gui}

Prima di poter aggiungere l'integrazione, devi prima avere un account con il partner che vuoi integrare.

{{site.data.keyword.security-advisor_short}} non salva in modo permanente alcuna credenziale correlata al servizio partner. Gli utenti aziendali devono autenticarsi utilizzando SAML sia per {{site.data.keyword.Bluemix_notm}} che per il business partner.
{: note}

### Configurazione della connessione
{: #custom-configure-connection}

1. Accedi al tuo strumento di sicurezza e ottieni il tuo URL univoco.

2. Accedi a {{site.data.keyword.cloud_notm}} utilizzando la console.

3. Fai clic su **Custom Integrations > Direct Connection**. Viene visualizzata una schermata.

  1. Fornisci un nome alla tua soluzione. Nel nome puoi utilizzare solo caratteri alfanumerici, spazi vuoti e trattini (-).

  2. Immetti l'URL per la soluzione nel formato: `www.<website>.<domain>`.

  3. Carica un'icona o un'immagine per rappresentare lo strumento.

  4. Fai clic su **Connect** per completare la configurazione. {{site.data.keyword.security-advisor_short}} crea le risorse richieste per l'integrazione come l'ID servizio, la chiave API, l'ID account e i metadati. Viene assegnato il ruolo `writer`.



## Integra le ricerche di terze parti

Le API seguono Grafeas come specifiche dei metadati della risorsa per archiviare, eseguire la query e richiamare i metadati critici per le ricerche segnalate dai tuoi servizi e strumenti di sicurezza.
{: #setup-custom-api}

### Prima di cominciare
{: #custom-before-api}

Prima di integrare le ricerche dal tuo strumento di terze parti, assicurati di avere i seguenti prerequisiti.

1. Assicurati che all'utente o all'ID servizio che stai utilizzando sia stato assegnato il [ruolo IAM](https://cloud.ibm.com/iam#/users) di **Gestore**.

1. Accedi a {{site.data.keyword.cloud_notm}}.

  ```
  ibmcloud login
  ```
  {: codeblock}

2. Ottieni il tuo ID account. Per ulteriori informazioni sui ruoli del servizio, controlla le [politiche di accesso {{site.data.keyword.security-advisor_short}}](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

3. Ottieni il tuo token Identity and Access Management (IAM). Il token viene utilizzato in `--header` di ogni richiesta API.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  I token IAM scadono ogni 60 minuti. Impara come [ottenere automaticamente un nuovo token](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey) utilizzando una chiave API.
  {: tip}



### Importazione delle ricerche e dei KRI
{: #custom-adding}

1. Registra un nuovo tipo di ricerca creando una nota. Per creare la nota, utilizza [Findings API](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote). Assicurati di scegliere un ID del provider univoco per identificare il tuo strumento personalizzato. Se stai automatizzando il processo utilizzando un ID del servizio, lo puoi utilizzare come tuo ID del provider.

  Richiesta di esempio:

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Content-Type: application/json" -d "{ \"kind\": \"FINDING\", \"short_description\": \"My security tool finding\", \"long_description\": \"See what my custom security tool found\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-findings-type\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Custom Security Tool\" }, \"finding\": { \"severity\": \"MEDIUM\", \"next_steps\": [ { \"title\": \"Learn why this is reported as a risk\" } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icona Ulteriori informazioni"/> Descrizioni di questi componenti del comando </th>
    </thead>
    <tbody>
      <tr>
        <td><code>note_name</code></td>
        <td><code>&lt;account id&gt;/providers/my-custom-tool/notes/my-custom-tool-findings-type</code></td>
      </tr>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>remediation</code></td>
        <td>I passi che occorre effettuare per risolvere il problema.</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>Il tuo strumento di sicurezza personalizzato.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>Un ID per il tipo di ricerca trovata dal tuo strumento di sicurezza.</td>
      </tr>
      <tr>
        <td><code>context</code><ul><li><code>region</code></li><li><code>resource_id</code></li> <li><code>resource_name</code></li> <li><code>resource_type</code></li> <li><code>service_name</code></li></ul></td>
        <td></br><ul><li><code>L'ubicazione in cui si è verificata la ricerca</code></li><li><code>L'ID per la risorsa specifica.</code></li> <li><code>Il nome della risorsa.</code></li> <li><code>Il tipo di risorsa.</code></li> <li><code>Il nome del servizio.</code></li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>url</code></li></ul></td>
        <td></br><ul><li>Il livello di urgenza che la ricerca presenta.</li> <li>I passi che possono essere effettuati per correggere il problema.</li> <li>Un URL dove possono essere trovati i dettagli della ricerca.</li></ul></td>
      </tr>
    </tbody>
  </table>

  Risposta di esempio:

  ```
    {
    "author": {
      "account_id": "account id",
      "email": "email id",
      "id": "IBM ID",
      "kind": "user"
    },
    "context": {
      "account_id": "account id"
    },
    "create_time": "2018-09-04T10:57:56.913871Z",
    "create_timestamp": 1536058676914,
    "finding": {
      "next_steps": [
        {
          "title": "Learn why this is reported as a risk"
        }
      ],
      "severity": "MEDIUM"
    },
    "id": "my-custom-tool-findings-type",
    "kind": "FINDING",
    "long_description": "See what my custom security tool found",
    "name": "<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
    "provider_id": "my-custom-tool",
    "provider_name": "<account id>/providers/my-custom-tool",
    "reported_by": {
      "id": "my-custom-tool",
      "title": "My Custom Security Tool"
    },
    "shared": true,
    "short_description": "My security tool finding",
    "update_time": "2018-09-04T10:57:56.913890Z",
    "update_timestamp": 1536058676914,
    "update_week_date": "2018-W36-2"
  }
  ```
  {: screen}

  Assicurati di ricordare il nome della nota che è stata restituita come parte della risposta. In questo esempio, il valore è `/providers/my-custom-tool/notes/my-custom-tool-findings-type`. Questo valore viene utilizzato nel passo successivo.
  {: tip}

2. Inserisci le ricerche come KRI o eventi, note anche come una [ricorrenza](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence).

  Per ogni scheda, puoi definire due KRI.
  {: note}

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account-id>/providers/my-custom-tool/occurrences" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Replace-If-Exists: true" -H "Content-Type: application/json" -d "{ \"note_name\": \"<account-id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\", \"kind\": \"FINDING\", \"remediation\": \"how to resolve this\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-finding-1\", \"context\": { \"region\": \"location\", \"resource_id\": \"pluginId\", \"resource_name\": \"www.myapp.com\", \"resource_type\": \"worker\", \"service_name\": \"application\" }, \"finding\": { \"severity\": \"HIGH\", \"next_steps\": [{ \"url\": \"Details URL\" }] }}"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icona Ulteriori informazioni"/> Descrizioni di questi componenti del comando </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>Una breve descrizione che riassume la ricerca; non più di un paio di parole.</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>Una descrizione lunga che contiene più dettagli sulla ricerca.</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>Il tuo strumento di sicurezza personalizzato.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>Un ID per il tipo di ricerca trovata dal tuo strumento di sicurezza.</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>L'ID dello strumento di sicurezza che ha segnalato la ricerca.</li><li>Il titolo dello strumento di sicurezza che ha segnalato la ricerca.</li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>title</code></li></ul></td>
        <td></br><ul><li>Il livello di urgenza che la ricerca presenta.</li> <li>I passi che possono essere effettuati per correggere il problema.</li> <li>Il titolo della ricerca.</li></ul></td>
      </tr>
    </tbody>
  </table>

  Risposta di esempio:

  ```
    {
    "author": {
      "account_id": "account id",
      "email": "email id ",
      "id": "user id",
      "kind": "user"
    },
    "context": {
      "account_id": "account id",
      "region": "location",
      "resource_id": "pluginId",
      "resource_name": "www.myapp.com",
      "resource_type": "worker",
      "service_name": "application"
    },
    "create_time": "2018-09-04T11:32:14.564809Z",
    "create_timestamp": 1536060734565,
    "finding": {
      "next_steps": [
        {
          "title": "Learn why this is reported as a risk",
          "url": "Details URL"
        }
      ],
      "severity": "HIGH"
    },
    "id": "my-custom-tool-finding-1",
    "kind": "FINDING",
    "long_description": "See what my custom security tool found",
    "name": "<account id>/providers/my-custom-tool/occurrences/my-custom-tool-finding-1",
    "note_name": "<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
    "provider_id": "my-custom-tool",
    "provider_name": "<account id>/providers/my-custom-tool",
    "remediation": "how to resolve this",
    "reported_by": {
      "id": "my-custom-tool",
      "title": "My Custom Security Tool"
    },
    "short_description": "My security tool finding",
    "update_time": "2018-09-04T11:32:14.564828Z",
    "update_timestamp": 1536060734565,
    "update_week_date": "2018-W36-2"
  }
  ```
  {: screen}

3. Definisci la scheda nel dashboard per visualizzare la tua ricerca creando una [nota](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote).

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam token>" -H "Content-Type: application/json" -d "{ \"kind\": \"CARD\", \"provider_id\": \"my-custom-tool\", \"id\": \"custom-tool-card\", \"short_description\": \"security risk found by my custom tool\", \"long_description\": \"Details about why this is security risk to be fixed\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Security Tool\" }, \"card\": { \"section\": \"My Security Tools\", \"title\": \"My Security Tool Findings\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ], \"elements\": [ { \"kind\": \"NUMERIC\", \"text\": \"Count of findings reported by my security tool\", \"default_time_range\": \"1d\", \"value_type\": { \"kind\": \"FINDING_COUNT\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ] } } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icona Ulteriori informazioni"/> Descrizioni di questi componenti del comando </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>CARD</code></td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>Il tuo strumento di sicurezza personalizzato.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>Un ID per il tipo di ricerca trovata dal tuo strumento di sicurezza.</td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>Una descrizione, non più di un paio di parole, che riassume la ricerca.</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>Una descrizione lunga che contiene più dettagli sulla ricerca.</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>L'ID dello strumento di sicurezza che ha segnalato la ricerca.</li><li>Il titolo dello strumento di sicurezza che ha segnalato la ricerca.</li></ul></td>
      </tr>
      <tr>
        <td><code>card</code> <ul><li><code>section</code></li> <li><code>title</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>La sezione in cui si inserisce la scheda.</li> <li>Il titolo della scheda</li> <li><code>providers/<provider_id>/notes/my-custom-tool-findings-type</code></li></ul></td>
      </tr>
      <tr>
        <td><code>elements</code> <ul><li><code>kind</code></li> <li><code>text</code></li> <li><code>default_time_range</code></li></ul></td>
        <td></br><ul><li>Il tipo di elemento.</li> <li>Il testo che vuoi visualizzare</li> <li>La quantità di tempo che vuoi verificare.</li></ul></td>
      </tr>
      <tr>
        <td><code>value_type</code> <ul><li><code>kind</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>Il tipo di valore</li> <li>Il nome delle ricerche che vuoi visualizzare nella scheda.</li></ul></td>
      </tr>
    </tbody>
  </table>

  Risposta di esempio:
  ```
    {
    "author": {
      "account_id": "<account id",
      "email": "email id",
      "id": "user id",
      "kind": "user"
    },
    "card": {
      "elements": [
        {
          "default_time_range": "1d",
          "kind": "NUMERIC",
          "text": "Count of findings reported by my security tool",
          "value_type": {
            "finding_note_names": [
              "providers/my-custom-tool/notes/my-custom-tool-findings-type"
            ],
            "kind": "FINDING_COUNT"
          }
        }
      ],
      "finding_note_names": [
        "providers/my-custom-tool/notes/my-custom-tool-findings-type"
      ],
      "section": "My Security Tools",
      "title": "My Security Tool Findings"
    },
    "context": {
      "account_id": "<account id>"
    },
    "create_time": "2018-09-04T11:49:36.056047Z",
    "create_timestamp": 1536061776056,
    "id": "custom-tool-card",
    "kind": "CARD",
    "long_description": "Details about why this is security risk to be fixed",
    "name": "<account id>/providers/my-custom-tool/notes/custom-tool-card",
    "provider_id": "my-custom-tool",
    "provider_name": "<account id>/providers/my-custom-tool",
    "reported_by": {
      "id": "my-custom-tool",
      "title": "My Security Tool"
    },
    "shared": true,
    "short_description": "security risk found by my custom tool",
    "update_time": "2018-09-04T11:49:36.056066Z",
    "update_timestamp": 1536061776056,
    "update_week_date": "2018-W36-2"
  }
  ```
  {: screen}

4. Passa al tuo dashboard del servizio per visualizzare la scheda che hai creato.

## Utilizzo di esempio
{: #custom-example}

Diciamo che hai un'applicazione eseguita in un cluster {{site.data.keyword.containershort_notm}} con il nome `cloudkingdom`. A seconda della dimensione della tua applicazione, potresti avere diversi pod all'interno del tuo cluster per monitorare tutto contemporaneamente. E se hai più strumenti personalizzati che monitorano e rilevano il tuo cluster per minacce diverse. Se uno dei tuoi pod nel cluster inizia ad inviare una quantità anomala di dati a server esterni, potresti volerlo sapere il prima possibile. Lo strumento personalizzato che monitora il trasferimento di dati, può rilevare la ricerca e inviarla a {{site.data.keyword.security-advisor_short}}. Se hai un'altra integrazione personalizzata che rileva un problema, anche lei invierebbe la ricerca a {{site.data.keyword.security-advisor_short}}. Pertanto, {{site.data.keyword.security-advisor_short}} visualizza le ricerche da tutti gli strumenti di monitoraggio in un solo dashboard. Qui puoi velocemente vedere una panoramica di tutti gli avvisi, investigare sul problema e imparare come apportare le correzioni.


Payload di esempio:

```
{
	"note_name": "<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
	"kind": "FINDING",
	"remediation": "How to resolve Data leakage threat",
	"provider_id": "my-custom-tool",
	"id": "my-custom-tool-finding-2",
	"context": {
		"region": "location",
		"resource_id": "cluster crn",
		"resource_name": "cloudkingdom",
		"resource_type": "container",
		"service_name": "kubernetes service"
	},
	"finding": {
		"severity": "HIGH",
		"next_steps": [{
			"title": "Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities",
                        "url":"https://console.bluemix.net/containers-kubernetes/clusters"
		}],
                "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
                "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod’s normal behavior"
	}
}
```
{: screen}
