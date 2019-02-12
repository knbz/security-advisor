---

copyright:
  years: 2018
lastupdated: "2018-12-09"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Strumenti di sicurezza personalizzati
{: #setup_custom}

Ci sono volte in cui potresti già avere degli strumenti che usi. Puoi integrare quegli strumenti come, Neuvector, con {{site.data.keyword.security-advisor_short}}.
{: shortdesc}


Perché dovresti voler creare le personalizzazioni? Diciamo che hai un'applicazione in esecuzione in un cluster {{site.data.keyword.containershort_notm}} con il nome `cloudkingdom`. Uno dei pod nel cluster sta inviando una quantità anomala di dati ai server esterni. Vuoi acquisire questa ricerca nel tuo dashboard {{site.data.keyword.security-advisor_short}}.

Se il tuo strumento personalizzato monitora e rileva la quantità anomala di dati che sta venendo trasferita, lo strumento invia la ricerca a {{site.data.keyword.security-advisor_short}}.

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
		"service_name": "kubernetess service"
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

</br>


## Integrazione dei tuoi strumenti con la GUI
{: #gui}

Puoi integrare i tuoi strumenti di sicurezza utilizzando il dashboard {{site.data.keyword.security-advisor_short}}.
{: shortdesc}

**Prima di cominciare**

* Devi disporre di un account con il partner che vuoi integrare.

{{site.data.keyword.security-advisor_short}} non salva in modo permanente alcuna credenziale correlata al servizio partner. Gli utenti aziendali devono autenticarsi utilizzando SAML sia per {{site.data.keyword.Bluemix_notm}} che per il business partner.
{: note}

1. Accedi al tuo strumento di sicurezza e ottieni il tuo URL univoco.
2. Accedi a {{site.data.keyword.Bluemix_notm}}.
3. Fai clic su **Custom Integrations** e quindi sulla scheda **Add Custom Solution**. Viene visualizzata una schermata.
  1. Fornisci un nome alla tua soluzione. Puoi utilizzare solo caratteri alfanumerici, spazi vuoti e trattini (-).
  2. Immetti l'URL per la soluzione nel formato: `www.<website>.<domain>`.
  3. Carica un'icona o un'immagine per rappresentare lo strumento.

    {{site.data.keyword.security-advisor_short}} crea le risorse richieste per l'integrazione come l'ID servizio, la chiave API, l'ID account e i metadati. Viene assegnato il ruolo `writer`.

4. Puoi configurare l'account cliente nella scheda **Integration** dell'altro servizio.
5. Con l'integrazione in atto, puoi iniziare a pubblicare le ricorrenze in {{site.data.keyword.security-advisor_short}} e a visualizzare le ricerche nel dashboard del servizio.

</br>

## Integrazione dei tuoi strumenti con l'API
{: #integrate}

Le API {{site.data.keyword.security-advisor_short}} seguono [Grafeas](https://grafeas.io/) come specifica dell'API dei metadati della risorsa per archiviare, eseguire la query e richiamare i metadati critici delle ricerche segnalati da tutti i servizi e gli strumenti di sicurezza.

**Prima di cominciare**

1. Accedi a {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud login
  ```
  {: codeblock}

2. Ottieni il tuo ID account. Assicurati che al tuo ID sia assegnato il [ruolo IAM](https://console.bluemix.net/iam/#/users) di **Gestore **. Per ulteriori informazioni sui ruoli del servizio, controlla le [politiche di accesso {{site.data.keyword.security-advisor_short}}](/docs/services/security-advisor/iam.html).

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

3. Ottieni un token IAM. Il token viene utilizzato in `--header` di ogni richiesta API.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

</br>

**Aggiunta e monitoraggio delle ricerche**

1. Registra un nuovo tipo di ricerca creando una nota. Per creare la nota, utilizza [Findings API](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote).

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

2. Crea una ricerca inviando una [ricorrenza](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence).

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
  {: codeblock}

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


</br>
</br>
