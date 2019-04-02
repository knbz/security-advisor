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


# Personnalisation 
{: #setup_custom}

{{site.data.keyword.security-advisor_short}} vous permet d'intégrer vos outils de sécurité personnalisés existants, qu'il s'agisse de services open source, que vous avez développés ou tiers. Vous pouvez créer une connexion directe de {{site.data.keyword.security-advisor_short}} à l'autre outil en ajoutant l'URL à votre liste d'intégrations. Vous pouvez également intégrer des résultats de tiers pour afficher les événements de sécurité critiques directement sur une nouvelle carte dans le tableau de bord en utilisant les API pour configurer des indicateurs clés de risques.
{: shortdesc}


## Ajout d'une connexion directe
{: #setup-custom-gui}

Vous pouvez assurer facilement le suivi de vos outils de sécurité dans le tableau de bord {{site.data.keyword.security-advisor_short}}.
{: shortdesc}

### Avant de commencer
{: #custom-before-gui}

Pour pouvoir ajouter l'intégration, vous devez disposer d'un compte avec le partenaire que vous voulez intégrer. 

{{site.data.keyword.security-advisor_short}} ne conserve pas les données d'identification associées au service partenaire. Les utilisateurs de l'entreprise doivent s'authentifier à l'aide de SAML auprès d'{{site.data.keyword.Bluemix_notm}} et du partenaire commercial.
{: note}

### Configuration de la connexion
{: #custom-configure-connection}

1. Connectez-vous à votre outil de sécurité et obtenez votre URL unique.

2. Connectez-vous à {{site.data.keyword.cloud_notm}} depuis la console. 

3. Cliquez sur **Intégrations personnalisées > Direct Connection**. Un écran s'affiche.

  1. Attribuez un nom à votre solution. Le nom ne peut comporter que des caractères alphanumériques, des espaces et des traits d'union. 

  2. Entrez l'URL de la solution au format `www.<website>.<domain>`.

  3. Téléchargez une icône ou une image pour représenter l'outil.

  4. Cliquez sur **Connecter** pour finaliser la configuration. {{site.data.keyword.security-advisor_short}} crée les artefacts requis pour l'intégration, tels que l'ID de service, la clé d'API, l'ID de compte et les métadonnées. Le rôle `Auteur` est affecté.



## Intégration de résultats de tiers 

Les API suivent les spécifications d'API de métadonnées d'artefact de type Grafeas pour stocker, interroger et extraire les métadonnées critiques pour les résultats rapportés par vos outils de sécurité et services.
{: #setup-custom-api}

### Avant de commencer
{: #custom-before-api}

Avant d'intégrer des résultats provenant de votre outil tiers, assurez-vous de disposer des prérequis suivants : 

1. Assurez-vous que l'ID utilisateur ou l'ID de service que vous utilisez possède le [rôle IAM](https://cloud.ibm.com/iam#/users) **Gestionnaire**.

1. Connectez-vous à {{site.data.keyword.cloud_notm}}.

  ```
  ibmcloud login
  ```
  {: codeblock}

2. Obtenez votre ID de compte. Pour plus d'informations sur les rôles de service, voir les [règles d'accès de {{site.data.keyword.security-advisor_short}}](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

3. Obtenez votre jeton Identity and Access Management (IAM). Ce jeton est utilisé dans l'en-tête (`--header`) de chaque demande d'API.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  Les jetons expirent toutes les 60 minutes. Découvrez comment [obtenir un nouveau jeton automatiquement](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey) à l'aide d'une clé d'API.
  {: tip}



### Importation de résultats et d'indicateurs clés de risques 
{: #custom-adding}

1. Enregistrez un nouveau type de résultat en créant une note. Pour ce faire, utilisez l'[API Findings](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote). Assurez-vous de choisir un ID de fournisseur unique afin d'identifier votre outil personnalisé. Si vous automatisez le processus avec un ID de service, vous pouvez utiliser l'ID de service comme ID de fournisseur. 

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
        <td><code>note_name</code></td>
        <td><code>&lt;ID compte&gt;/providers/my-custom-tool/notes/my-custom-tool-findings-type</code></td>
      </tr>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>remediation</code></td>
        <td>Etapes à effectuer pour résoudre le problème.</td>
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
        <td></br><ul><li>Niveau d'urgence que présente le résultat.</li> <li>Etapes pouvant être effectuées pour résoudre le problème.</li> <li>URL correspondant aux détails du résultat.</li></ul></td>
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
  {: screen}

  Mémorisez le nom de la note qui est renvoyée dans la réponse. Dans cet exemple, il s'agit de `/providers/my-custom-tool/notes/my-custom-tool-findings-type`. Cette valeur est utilisée à l'étape suivante.
  {: tip}

2. Publiez des résultats sous forme d'indicateurs clés de risques ou d'événements, aussi appelés [occurrences](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence).

  Pour chaque carte, vous pouvez définir deux indicateurs clés de risques.
  {: note}

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
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>Brève description qui récapitule le résultat (un ou deux mots maximum).</td>
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
        <td></br><ul><li>ID de l'outil de sécurité qui a rapporté le résultat.</li><li>Titre de l'outil de sécurité qui a rapporté le résultat.</li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>title</code></li></ul></td>
        <td></br><ul><li>Niveau d'urgence que présente le résultat.</li> <li>Etapes pouvant être effectuées pour résoudre le problème.</li> <li>Titre du résultat.</li></ul></td>
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
  {: screen}

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
        <td>Description qui récapitule le résultat (un ou deux mots maximum).</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>Description plus longue qui comporte davantage de détails sur le résultat.</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>ID de l'outil de sécurité qui a rapporté le résultat.</li><li>Titre de l'outil de sécurité qui a rapporté le résultat.</li></ul></td>
      </tr>
      <tr>
        <td><code>card</code> <ul><li><code>section</code></li> <li><code>title</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>Section dans laquelle se trouve la carte. </li> <li>Titre de la carte.</li> <li><code>providers/<provider_id>/notes/my-custom-tool-findings-type</code></li></ul></td>
      </tr>
      <tr>
        <td><code>elements</code> <ul><li><code>kind</code></li> <li><code>text</code></li> <li><code>default_time_range</code></li></ul></td>
        <td></br><ul><li>Type de l'élément.</li> <li>Texte à afficher.</li> <li>Période de temps à vérifier.</li></ul></td>
      </tr>
      <tr>
        <td><code>value_type</code> <ul><li><code>kind</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>Type de valeur.</li> <li>Nom des résultats à afficher sur la carte.</li></ul></td>
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
  {: screen}

4. Accédez à votre tableau de bord du service pour voir la carte que vous avez créée.

## Exemple d'utilisation 
{: #custom-example}

Imaginez que vous disposez d'une application qui s'exécute dans un cluster {{site.data.keyword.containershort_notm}} dont le nom est `cloudkingdom`. Selon la taille de votre application, votre cluster peut comporter plusieurs pods que vous devez surveiller simultanément. Que se passe-t-il lorsque plusieurs outils personnalisés surveillent votre cluster et détectent différentes menaces ? Si l'un des pods dans le cluster commençait à envoyer une quantité anormale de données à des serveurs externes, vous voudriez en être informé le plus tôt possible. L'outil personnalisé qui surveille le transfert de données peut détecter le résultat et l'envoyer à {{site.data.keyword.security-advisor_short}}. Si une autre de vos intégrations personnalisées détecte un problème, elle envoie également le résultat à {{site.data.keyword.security-advisor_short}}. Ensuite, {{site.data.keyword.security-advisor_short}} affiche les résultats depuis tous vos outils de surveillance dans un seul tableau de bord. Là, vous pouvez rapidement prendre connaissance des alertes, examiner un problème, et en savoir plus sur les étapes de résolution. 


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
