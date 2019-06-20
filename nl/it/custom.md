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


# Ricerche personalizzate
{: #setup_custom}

Con {{site.data.keyword.security-advisor_short}}, puoi integrare i tuoi strumenti di sicurezza personalizzati esistenti, sia che siano servizi open source, sviluppati in modo personalizzato o di terze parti. Integrando le ricerche di terze parti, puoi portare tutti i tuoi strumenti e le ricerche di sicurezza in un unico dashboard in cui puoi monitorare gli eventi di sicurezza critici.
{: shortdesc}


## Prima di cominciare
{: #custom-before-api}

Prima di integrare le ricerche dal tuo strumento di terze parti, assicurati di avere i seguenti prerequisiti.

1. Assicurati che all'utente o all'ID servizio che stai utilizzando sia stato assegnato il [ruolo IAM](https://cloud.ibm.com/iam#/users) di **Gestore**.

2. Accedi a {{site.data.keyword.cloud_notm}}.

  ```
  ibmcloud login
  ```
  {: codeblock}

3. Ottieni il tuo ID account. Per ulteriori informazioni sui ruoli del servizio, controlla le [politiche di accesso {{site.data.keyword.security-advisor_short}}](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

4. Ottieni il tuo token Identity and Access Management (IAM). Il token viene utilizzato in `--header` di ogni richiesta API.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  I token IAM scadono ogni 60 minuti. Impara come [ottenere automaticamente un nuovo token](/docs/iam?topic=iam-iamtoken_from_apikey) utilizzando una chiave API.
  {: tip}



## Importazione delle ricerche e dei KRI
{: #custom-adding}

Le API seguono Grafeas come specifiche dei metadati della risorsa per archiviare, eseguire la query e richiamare i metadati critici per le ricerche segnalate dai tuoi servizi e strumenti di sicurezza. L'integrazione può essere eseguita utilizzando le API per configurare i KRI (key risk indicator).
{: shortdesc}


### Passo 1: registrazione di un nuovo tipo di ricerca
{: #custom-register-finding}

Per registrare un nuovo tipo di ricerca, puoi creare una nota. Per creare la nota, puoi utilizzare l'[API Findings](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}. Assicurati di scegliere un ID del provider univoco per identificare il tuo strumento personalizzato. Se stai automatizzando il processo utilizzando un ID servizio, lo puoi utilizzare come tuo ID provider.

Richiesta di esempio:

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{  \"kind\": \"FINDING\",  \"short_description\": \"My security tool finding\",  \"long_description\": \"Longer description of what the security tool found.\",  \"provder_id\": \"my-custom-tool\",  \"id\": \"my-custom-tool-findings-type\",  \"reported_by\": {    \"id\": \"my-custom-tool\",    \"title\": \"My custom security tool\"  } ,  \"finding\": {    \"severity\": \"MEDIUM\",    \"next_steps\": [      {        \"title\": \"Explain why it's reported as a risk.\"      }    ]  }}"
```
{: codeblock}

| Generale | Descrizione | 
|:-----------------|:-----------------|
| `note_name` | `<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type`  |
| `kind` | `FINDING` |
| `remediation` | I passi che occorre effettuare per risolvere il problema. |
| `provider_id` | Il tuo strumento di sicurezza personalizzato. |
| `id` | Un ID per il tipo di ricerca trovata dal tuo strumento di sicurezza. |
{: class="simple-tab-table"}
{: caption="Tabella 1. Descrizione dei componenti generali del comando" caption-side="top"}
{: #registerfindingtable1}
{: tab-title="General"}
{: tab-group="register"}

| Contesto | Descrizione | 
|:-----------------|:-----------------|
| `region` | L'ubicazione in cui si è verificata la ricerca.  |
| `resource_id` | L'ID per la risorsa. |
| `resource_name` | Il nome della risorsa. |
| `resource_type` | Il tipo di risorsa. |
| `service_name` | Il nome del servizio. |
{: class="simple-tab-table"}
{: caption="Tabella 2. Descrizione dei componenti di contesto del comando" caption-side="top"}
{: #registerfinding2}
{: tab-title="Context"}
{: tab-group="register"}

| Ricerca | Descrizione | 
|:-----------------|:-----------------|
| `severity` | Il livello di urgenza che la ricerca presenta.  |
| `next_steps` | I passi che possono essere effettuati per correggere il problema. |
| `url` | Un URL dove possono essere trovati i dettagli della ricerca. |
{: class="simple-tab-table"}
{: caption="Tabella 3. Descrizione dei componenti di ricerca del comando" caption-side="top"}
{: #registerfinding3}
{: tab-title="Finding"}
{: tab-group="register"}

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


### Passo 2: pubblicazione delle ricerche
{: #custom-post-findings}

Crea una [ricorrenza](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence){: external} per pubblicare le ricerche come KRI o eventi sul tuo dashboard di Security Advisor.

Per ogni scheda, puoi definire due KRI.
{: note}

Richiesta di esempio: 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/occurrences"  -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"note_name\": \"<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\",    \"kind\": \"FINDING\",    \"remediation\": \"How to resolve Data leakage threat\",    \"provider_id\": \"my-custom-tool\",    \"id\": \"my-custom-tool-finding-2\",    \"context\": {        \"region\": \"location\",        \"resource_id\": \"cluster crn\",        \"resource_name\": \"cloudkingdom\",        \"resource_type\": \"container\",        \"service_name\": \"kubernetes service\"    },    \"finding\": {        \"severity\": \"HIGH\",        \"next_steps\": [{            \"title\": \"Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities\",                        \"url\":\"https://cloud.ibm.com/containers-kubernetes/clusters\"        }],                \"short_description\": \"One of the pods in your cluster appears to be leaking an excessive amount of data\",                \"long_description\": \"One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod's normal behavior\"    }}"
```
{: codeblock}

| Generale | Descrizione | 
|:-----------------|:-----------------|
| `kind` | `FINDING` |
| `short_description` | Una breve descrizione che riassume la ricerca; non più di un paio di parole. |
| `long_description` | Una descrizione più lunga che contiene maggiori dettagli sulla ricerca. |
| `provider_id` | Il tuo strumento di sicurezza personalizzato. |
| `id` | Un ID per il tipo di ricerca trovata dal tuo strumento di sicurezza. |
{: class="simple-tab-table"}
{: caption="Tabella 2. Descrizione dei componenti generali del comando" caption-side="top"}
{: #postfinding1}
{: tab-title="General"}
{: tab-group="post"}

| Segnalato da | Descrizione | 
|:-----------------|:-----------------|
| `id` | L'ID dello strumento di sicurezza che ha segnalato la ricerca.  |
| `title` | Il titolo dello strumento di sicurezza che ha segnalato la ricerca. |
{: class="simple-tab-table"}
{: caption="Tabella 2. Descrizione dei componenti dello strumento di segnalazione del comando" caption-side="top"}
{: #postfinding2}
{: tab-title="Reported by"}
{: tab-group="post"}

| Ricerca | Descrizione | 
|:-----------------|:-----------------|
| `severity` | Il livello di urgenza che la ricerca presenta.  |
| `next_steps` | I passi che possono essere effettuati per correggere il problema. |
| `title` | Il titolo della ricerca. |
{: class="simple-tab-table"}
{: caption="Tabella 3. Descrizione dei componenti di ricerca del comando" caption-side="top"}
{: #postfinding3}
{: tab-title="Finding"}
{: tab-group="post"}


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

### Passo 3: definizione della scheda da visualizzare
{: #custom-define-card}

Definisci come vuoi che la tua scheda visualizzi le tue ricerche nel dashboard creando una [nota](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}.

Richiesta di esempio: 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"kind\": \"CARD\",        \"provider_id\": \"my-custom-tool\",    \"id\": \"custom-tool-card\",    \"short_description\": \"Security risk found by my custom tool\",        \"long_description\": \"More detailed description about why this security risk needs to be fixed\",    \"reported_by\": {      \"id\": \"my-custom-tool\",      \"title\": \"My security tool\"    },        \"card\": {      \"section\": \"My security tools\",      \"order\": 1 ,     \"title\": \"My security tool findings\",      \"subtitle\": \"My security tool\",      \"finding_note_names\": [        \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"      ],      \"elements\": [        {          \"kind\": \"NUMERIC\",          \"text\": \"Count of findings reported by my security tool\",          \"default_time_range\": \"1d\",          \"value_type\": {            \"kind\": \"FINDING_COUNT\",            \"finding_note_names\": [              \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"            ]          }        }      ]    }  }"
```
{: codeblock}

| Generale | Descrizione | 
|:-----------------|:-----------------|
| `provider_id` | L'ID del tuo strumento di sicurezza. |
| `id` | Un ID per il tipo di ricerca trovata dal tuo strumento di sicurezza. |
| `short_description` | Una breve descrizione che riassume la ricerca; non più di un paio di parole. |
| `long_description` | Una descrizione più lunga che contiene maggiori dettagli sulla ricerca. |
{: class="simple-tab-table"}
{: caption="Tabella 1. Descrizione dei componenti generali del comando" caption-side="top"}
{: #definecard1}
{: tab-title="General"}
{: tab-group="card"}

| Segnalato da | Descrizione | 
|:-----------------|:-----------------|
| `id` | L'ID dello strumento di sicurezza che ha segnalato la ricerca.  |
| `title` | Il titolo dello strumento di sicurezza che ha segnalato la ricerca. |
{: class="simple-tab-table"}
{: caption="Tabella 2. Descrizione dei componenti dello strumento di segnalazione del comando" caption-side="top"}
{: #definecard2}
{: tab-title="Reported by"}
{: tab-group="card"}

| Scheda | Descrizione | 
|:-----------------|:-----------------|
| `section` | La sezione in cui vuoi visualizzare la scheda. Puoi avere fino a tre sezioni personalizzate con sei schede in ogni sezione. Numero massimo di caratteri: 30  |
| Facoltativo: `order` | L'ordine in cui la scheda viene visualizzata all'interno della sezione specificata. L'ordine viene specificato nell'intervallo 1 - 6. Se scegli un numero che è già applicato a un'altra scheda, la creazione non riesce. Ricevi un messaggio di errore che indica "Given order is already taken by other card in section." Se l'ordine fornito è superiore al numero corrente di schede più 1, la creazione della scheda non riesce. Ad esempio, se al momento hai due schede e ne stai creando un'altra, non puoi specificare 5 nell'ordine delle schede perché, nell'insieme, hai un totale di tre schede. Se l'ordine per le schede non viene specificato, vengono disposte in ordine alfabetico nella sezione assegnata. |
| `title` | Il titolo che vuoi assegnare alla tua scheda. Numero massimo di caratteri: 28 |
| `subtitle` | Il sottotitolo che vuoi assegnare alla tua scheda. Numero massimo di caratteri: 30  |
| `finding_note_names` | `providers//notes/my-custom-tool-findings-type` |
{: class="simple-tab-table"}
{: caption="Tabella 3. Descrizione dei componenti di scheda del comando" caption-side="top"}
{: #definecard3}
{: tab-title="Card"}
{: tab-group="card"}

| Elementi: Tipo di valore | Descrizione | 
|:-----------------|:-----------------|
| `kind` | Le opzioni includono: `NUMERIC`, `TIME_SERIES` e `BREAKDOWN`. |
| `text` | Il testo che vuoi visualizzare. Se il tipo è `NUMERIC`, il numero massimo di caratteri è 60. Se il tipo è `TIME_SERIES` o `BREAKDOWN`, il numero massimo di caratteri è 65. |
| `default_time_range` | La quantità di tempo che vuoi verificare. I valori sono impostati in giorni. Le opzioni correnti includono: `1d`, `2d`, `3d` e `4d`. |
| `value_type` | Il tipo di elemento. Se il tipo è `NUMERIC`, il campo è `value_type` e puoi avere fino a quattro elementi per ogni scheda. Se il tipo è `TIME_SERIES` o `BREAKDOWN`, il campo è `value_types`. Il numero massimo di `TIME_SERIES` o `BREAKDOWN` è 1. Se hai solo voci numeriche, puoi avere fino a quattro elementi per ogni scheda. Se vuoi utilizzare una combinazione, puoi avere fino a due voci numeriche e una serie temporale o una suddivisione. Non puoi avere sia serie temporali che suddivisioni nella stessa scheda. Se definisci i tuoi tipi di valore come array per le serie temporali, puoi avere fino a tre voci.  |
| `value_type`: `kind` | Il tipo di valore. Le opzioni includono: `KRI` e `FINDING_COUNT`. |
| `value_type`: `finding_note_names` |Se `kind` è `FINDING_COUNT`, il nome delle ricerche che vuoi visualizzare nella tua scheda, che è specificato come array. |
| `value_type`: `kri_note_names` |Se `kind` è `FINDING_COUNT`, il nome delle ricerche che vuoi visualizzare nella tua scheda, che è specificato come array. |
| `value_type`: `text` | Il testo del tipo di elemento. Il numero massimo di caratteri è 22. |
{: class="simple-tab-table"}
{: caption="Tabella 4. Descrizione dei componenti di elemento del comando" caption-side="top"}
{: #definecard4}
{: tab-title="Elements"}
{: tab-group="card"}

Risposta di esempio:

```
{
"author": {
  "account_id": "account id",
      "email": "email id ",
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
  "order": "1",
  "title": "My Security Tool Findings",
  "subtitle": "My Security Tool",
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


## Utilizzo di esempio
{: #custom-example}

Diciamo che hai un'applicazione eseguita in un cluster {{site.data.keyword.containershort_notm}} con il nome `cloudkingdom`. A seconda della dimensione della tua applicazione, potresti avere diversi pod all'interno del tuo cluster che devi monitorare tutti contemporaneamente. E se hai più strumenti personalizzati che monitorano e rilevano il tuo cluster per minacce diverse. Se uno dei tuoi pod nel cluster inizia ad inviare una quantità anomala di dati a server esterni, potresti volerlo sapere il prima possibile. Lo strumento personalizzato che monitora il trasferimento di dati, può rilevare la ricerca e inviarla a {{site.data.keyword.security-advisor_short}}. Se hai un'altra integrazione personalizzata che rileva un problema, anche lei invierebbe la ricerca a {{site.data.keyword.security-advisor_short}}. Pertanto, {{site.data.keyword.security-advisor_short}} visualizza le ricerche da tutti gli strumenti di monitoraggio in un solo dashboard. Qui puoi velocemente vedere una panoramica di tutti gli avvisi, investigare sul problema e imparare come apportare le correzioni.

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
                        "url":"https://cloud.ibm.com/containers-kubernetes/clusters"
		}],
                "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
                "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod’s normal behavior"
	}
}
```
{: screen}
