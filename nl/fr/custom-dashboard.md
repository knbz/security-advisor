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

# Intégration de vos propres outils de sécurité
{: #custom}

{{site.data.keyword.security-advisor_long}} fournit une API findings qui peut être utilisée pour unifier et améliorer la gestion de la sécurité sur IBM Cloud. Vous pouvez utiliser les API Findings pour enregistrer de nouveaux types de résultats pour vos services partenaires et vos outils de sécurité personnalisés. Une fois les métadonnées enregistrées, les outils peuvent envoyer toute occurrence ou tout résultat sous forme d'indicateurs clé de performance et d'événements en utilisant les API. Les résultats sont affichés sous forme de carte distincte dans le tableau de bord {{site.data.keyword.security-advisor_short}}.
{: shortdesc}

Les API {{site.data.keyword.security-advisor_short}} suivent la spécification d'API de métadonnées d'artefact de type [Grafeas](https://grafeas.io/) pour stocker, interroger et extraire les métadonnées critiques pour les résultats consignés par tous les services et outils de sécurité.

## Intégration de vos propres outils
{: #integrate}

**Avant de commencer**

1. Connectez-vous à {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud login
  ```
  {: codeblock}

2. Obtenez votre ID compte. Vérifiez que le [rôle IAM](https://console.bluemix.net/iam/#/users) **Manager** est affecté à votre ID. Pour plus d'informations sur les rôles de service, consultez les [{{site.data.keyword.security-advisor_short}}politiques d'accès](/docs/services/security-advisor/iam.html).

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

3. Obtenez votre jeton IAM. Ce jeton est utilisé dans l'en-tête (`--header`) de chaque demande d'API.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

</br>

**Ajout et surveillance de résultats**

1. Enregistrez un nouveau type de résultat (Finding) en créant une note. Pour ce faire, utilisez l'[API Findings](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote).

  Exemple de demande :

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Content-Type: application/json" -d "{ \"kind\": \"FINDING\", \"short_description\": \"My security tool finding\", \"long_description\": \"See what my custom security tool found\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-findings-type\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Custom Security Tool\" }, \"finding\": { \"severity\": \"MEDIUM\", \"next_steps\": [ { \"title\": \"Learn why this is reported as a risk\" } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icône Plus d'informations"/> Explication des composants de cette commande </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>Brève description qui récapitule le résultat (un ou deux mots max.).</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>Description plus longue qui comporte davantage de détails sur le résultat.</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>Votre outil de sécurité personnalisé.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>ID pour le type de résultat trouvé par votre outil de sécurité.</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>ID de l'outil de sécurité qui a signalé le résultat. </li><li>Titre de l'outil de sécurité qui a signalé le résultat. </li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>title</code></li></ul></td>
        <td></br><ul><li>Niveau d'urgence que présente le résultat. </li> <li>Etapes pouvant être exécutées pour résoudre le problème. </li> <li>Titre du résultat.</li></ul></td>
      </tr>
    </tbody>
  </table>

  Exemple de réponse :

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
  {: codeblock}

2. Créez un résultat en publiant une [occurrence](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence).

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account-id>/providers/my-custom-tool/occurrences" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Replace-If-Exists: true" -H "Content-Type: application/json" -d "{ \"note_name\": \"<account-id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\", \"kind\": \"FINDING\", \"remediation\": \"how to resolve this\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-finding-1\", \"context\": { \"region\": \"location\", \"resource_id\": \"pluginId\", \"resource_name\": \"www.myapp.com\", \"resource_type\": \"worker\", \"service_name\": \"application\" }, \"finding\": { \"severity\": \"HIGH\", \"next_steps\": [{ \"url\": \"Details URL\" }] }}"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icône Plus d'informations"/> Explication des composants de cette commande </th>
    </thead>
    <tbody>
      <tr>
        <td><code>note_name</code></td>
        <td><code>&lt;ID compte&gt;/providers/my-custom-tool/notes/my-custom-tool-findings-type</code></td>
      </tr>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>remediation</code></td>
        <td>Etapes à prendre pour résoudre le problème.</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>Votre outil de sécurité personnalisé.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>ID pour le type de résultat trouvé par votre outil de sécurité.</td>
      </tr>
      <tr>
        <td><code>context</code><ul><li><code>region</code></li><li><code>resource_id</code></li> <li><code>resource_name</code></li> <li><code>resource_type</code></li> <li><code>service_name</code></li></ul></td>
        <td></br><ul><li><code>Emplacement où s'est produit le résultat.</code></li><li><code>ID de la ressource spécifique.</code></li> <li><code>Nom de la ressource.</code></li> <li><code>Type de ressource.</code></li> <li><code>Nom du service.</code></li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>url</code></li></ul></td>
        <td></br><ul><li>Niveau d'urgence que présente le résultat.</li> <li>Etapes pouvant être exécutées pour résoudre le problème. </li> <li>URL correspondant aux détails du résultat.</li></ul></td>
      </tr>
    </tbody>
  </table>

  Exemple de réponse :

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

3. Définissez la carte dans le tableau de bord pour afficher votre résultat en créant une [note](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote).

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam token>" -H "Content-Type: application/json" -d "{ \"kind\": \"CARD\", \"provider_id\": \"my-custom-tool\", \"id\": \"custom-tool-card\", \"short_description\": \"security risk found by my custom tool\", \"long_description\": \"Details about why this is security risk to be fixed\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Security Tool\" }, \"card\": { \"section\": \"My Security Tools\", \"title\": \"My Security Tool Findings\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ], \"elements\": [ { \"kind\": \"NUMERIC\", \"text\": \"Count of findings reported by my security tool\", \"default_time_range\": \"1d\", \"value_type\": { \"kind\": \"FINDING_COUNT\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ] } } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icône Plus d'informations"/> Explication des composants de cette commande </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>CARD</code></td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>Votre outil de sécurité personnalisé.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>ID pour le type de résultat trouvé par votre outil de sécurité.</td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>Description, un ou deux mots au maximum, qui récapitule le résultat.</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>Description plus longue qui comporte davantage de détails sur le résultat.</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>ID de l'outil de sécurité qui a signalé le résultat. </li><li>Titre de l'outil de sécurité qui a signalé le résultat. </li></ul></td>
      </tr>
      <tr>
        <td><code>card</code> <ul><li><code>section</code></li> <li><code>title</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>Section à laquelle correspond la carte. </li> <li>Titre de la carte.</li> <li><code>providers/<provider_id>/notes/my-custom-tool-findings-type</code></li></ul></td>
      </tr>
      <tr>
        <td><code>elements</code> <ul><li><code>kind</code></li> <li><code>text</code></li> <li><code>default_time_range</code></li></ul></td>
        <td></br><ul><li>Type de l'élément. </li> <li>Texte à afficher.</li> <li>Période de temps à vérifier.</li></ul></td>
      </tr>
      <tr>
        <td><code>value_type</code> <ul><li><code>kind</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>Type de valeur.</li> <li>Nom des résultats à afficher dans la carte.</li></ul></td>
      </tr>
    </tbody>
  </table>

  Exemple de réponse :
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
  {: codeblock}

4. Accédez à votre tableau de bord de service pour voir la carte que vous avez créée. 

## Exemple de scénario
{: #example}

Pourquoi souhaiteriez-vous créer des personnalisations ? Disons que vous possédez une application qui exécute un cluster {{site.data.keyword.containershort_notm}} et qui porte le nom de `cloudkingdom`. L'un des pods du cluster envoie une quantité anormale de données à des serveurs externes. Vous souhaitez capture ce résultat dans votre tableau de bord {{site.data.keyword.security-advisor_short}}.

Si votre outil personnalisé surveille et détecte la quantité anormale de données qui sont transférées, alors il envoie le résultat à {{site.data.keyword.security-advisor_short}}.

Exemple de contenu :

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
{: codeblock}
