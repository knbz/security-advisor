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


# Résultats personnalisés
{: #setup_custom}

Avec {{site.data.keyword.security-advisor_short}}, vous pouvez intégrer vos outils de sécurité personnalisés existants, qu'il s'agisse de services open source, de services tiers ou de services développés sur mesure. En intégrant les résultats tiers, vous pouvez regrouper tous vos outils de sécurité et vos résultats dans un seul tableau de bord où vous pouvez surveiller les événements de sécurité critiques.
{: shortdesc}


## Avant de commencer
{: #custom-before-api}

Avant d'intégrer les résultats de votre outil tiers, assurez-vous que les conditions préalables suivantes sont remplies.

1. Assurez-vous que l'ID utilisateur ou l'ID de service que vous utilisez possède le [rôle IAM](https://cloud.ibm.com/iam#/users) **Gestionnaire**.

2. Connectez-vous à {{site.data.keyword.cloud_notm}}.

  ```
  ibmcloud login
  ```
  {: codeblock}

3. Obtenez votre ID de compte. Pour plus d'informations sur les rôles de service, voir les [règles d'accès de {{site.data.keyword.security-advisor_short}}](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

4. Obtenez votre jeton Identity and Access Management (IAM). Ce jeton est utilisé dans l'en-tête (`--header`) de chaque demande d'API.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  Les jetons expirent toutes les 60 minutes. Découvrez comment [obtenir un nouveau jeton automatiquement](/docs/iam?topic=iam-iamtoken_from_apikey) à l'aide d'une clé d'API.
  {: tip}



## Importation de résultats et d'indicateurs clés de risques
{: #custom-adding}

Les API suivent les spécifications d'API de métadonnées d'artefact de type Grafeas pour stocker, interroger et extraire les métadonnées critiques pour les résultats rapportés par vos outils de sécurité et services. L'intégration peut se faire en utilisant les API pour configurer les indicateurs clés de risque (KRI).
{: shortdesc}


### Etape 1 : Enregistrement d'un nouveau type de résultat
{: #custom-register-finding}

Pour enregistrer un nouveau type de résultats, vous pouvez créer une note. Pour créer la note, vous pouvez utiliser l'[API Findings](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}. Assurez-vous de choisir un ID de fournisseur unique afin d'identifier votre outil personnalisé. Si vous automatisez le processus, utilisez votre ID de service comme ID de fournisseur. 

Exemple de demande :

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{  \"kind\": \"FINDING\",  \"short_description\": \"My security tool finding\",  \"long_description\": \"Longer description of what the security tool found.\",  \"provder_id\": \"my-custom-tool\",  \"id\": \"my-custom-tool-findings-type\",  \"reported_by\": {    \"id\": \"my-custom-tool\",    \"title\": \"My custom security tool\"  } ,  \"finding\": {    \"severity\": \"MEDIUM\",    \"next_steps\": [      {        \"title\": \"Explain why it's reported as a risk.\"      }    ]  }}"
```
{: codeblock}

| Général | Description | 
|:-----------------|:-----------------|
| `note_name` | `<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type`  |
| `kind` | `FINDING` |
| `remediation` | Etapes à effectuer pour résoudre le problème. |
| `provider_id` | Votre outil de sécurité personnalisé. |
| `id` | ID du type de résultat trouvé par votre outil de sécurité. |
{: class="simple-tab-table"}
{: caption="Tableau 1. Description des composants généraux de la commande" caption-side="top"}
{: #registerfindingtable1}
{: tab-title="General"}
{: tab-group="register"}

| Contexte | Description | 
|:-----------------|:-----------------|
| `region` | Emplacement dans lequel le résultat s'est produit.  |
| `resource_id` | ID de la ressource. |
| `resource_name` | Nom de la ressource. |
| `resource_type` | Type de ressource. |
| `service_name` | Nom du service. |
{: class="simple-tab-table"}
{: caption="Tableau 2. Description des composants de contexte de la commande" caption-side="top"}
{: #registerfinding2}
{: tab-title="Context"}
{: tab-group="register"}

| Résultat | Description | 
|:-----------------|:-----------------|
| `severity` | Niveau d'urgence que présente le résultat.  |
| `next_steps` | Etapes à effectuer pour résoudre le problème. |
| `url` | URL correspondant aux détails du résultat. |
{: class="simple-tab-table"}
{: caption="Tableau 3. Description des composants des résultats de la commande" caption-side="top"}
{: #registerfinding3}
{: tab-title="Finding"}
{: tab-group="register"}

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


### Etape 2 : Publication des résultats
{: #custom-post-findings}

Créez une [occurrence](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence){: external} pour poster les résultats en tant que KRI ou événements sur votre tableau de bord Security Advisor.

Pour chaque carte, vous pouvez définir deux indicateurs clés de risques.
{: note}

Exemple de demande : 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/occurrences"  -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"note_name\": \"<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\",    \"kind\": \"FINDING\",    \"remediation\": \"How to resolve Data leakage threat\",    \"provider_id\": \"my-custom-tool\",    \"id\": \"my-custom-tool-finding-2\",    \"context\": {        \"region\": \"location\",        \"resource_id\": \"cluster crn\",        \"resource_name\": \"cloudkingdom\",        \"resource_type\": \"container\",        \"service_name\": \"kubernetes service\"    },    \"finding\": {        \"severity\": \"HIGH\",        \"next_steps\": [{            \"title\": \"Investigate which process are running in your cluster. Si vous suspectez que l'un de vos pods a été piraté, redémarrez-le et recherchez des vulnérabilités d'image.\",                        \"url\":\"https://cloud.ibm.com/containers-kubernetes/clusters\"        }],                \"short_description\": \"One of the pods in your cluster appears to be leaking an excessive amount of data\",                \"long_description\": \"One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod's normal behavior\"    }}"
```
{: codeblock}

| Général | Description | 
|:-----------------|:-----------------|
| `kind` | `FINDING` |
| `short_description` |Brève description qui récapitule le résultat (un ou deux mots maximum).|
| `long_description` |Description plus longue qui fournit davantage de détails sur le résultat.|
| `provider_id` | Votre outil de sécurité personnalisé. |
| `id` | ID du type de résultat trouvé par votre outil de sécurité. |
{: class="simple-tab-table"}
{: caption="Tableau 2. Description des composants généraux de la commande" caption-side="top"}
{: #postfinding1}
{: tab-title="General"}
{: tab-group="post"}

| Signalé par | Description | 
|:-----------------|:-----------------|
| `id` | ID de l'outil de sécurité qui a signalé le résultat.  |
| `title` | Titre de l'outil de sécurité qui a signalé le résultat. |
{: class="simple-tab-table"}
{: caption="Tableau 2. Description des composants signalés de la commande" caption-side="top"}
{: #postfinding2}
{: tab-title="Reported by"}
{: tab-group="post"}

| Résultat | Description | 
|:-----------------|:-----------------|
| `severity` | Niveau d'urgence que présente le résultat.  |
| `next_steps` | Etapes à effectuer pour résoudre le problème. |
| `title` | Titre du résultat. |
{: class="simple-tab-table"}
{: caption="Tableau 3. Description des composants des résultats de la commande" caption-side="top"}
{: #postfinding3}
{: tab-title="Finding"}
{: tab-group="post"}


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

### Etape 3 : Définition de la carte à afficher
{: #custom-define-card}

Définissez comment vous souhaitez que votre carte affiche vos résultats dans votre tableau de bord en créant une [note](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}.

Exemple de demande : 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"kind\": \"CARD\",        \"provider_id\": \"my-custom-tool\",    \"id\": \"custom-tool-card\",    \"short_description\": \"Security risk found by my custom tool\",        \"long_description\": \"More detailed description about why this security risk needs to be fixed\",    \"reported_by\": {      \"id\": \"my-custom-tool\",      \"title\": \"My security tool\"    },        \"card\": {      \"section\": \"My security tools\",      \"order\": 1 ,     \"title\": \"My security tool findings\",      \"subtitle\": \"My security tool\",      \"finding_note_names\": [        \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"      ],      \"elements\": [        {          \"kind\": \"NUMERIC\",          \"text\": \"Count of findings reported by my security tool\",          \"default_time_range\": \"1d\",          \"value_type\": {            \"kind\": \"FINDING_COUNT\",            \"finding_note_names\": [              \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"            ]          }        }      ]    }  }"
```
{: codeblock}

| Général | Description | 
|:-----------------|:-----------------|
| `provider_id` | ID de votre outil de sécurité. |
| `id` | ID du type de résultat trouvé par votre outil de sécurité. |
| `short_description` |Brève description qui récapitule le résultat (un ou deux mots maximum).|
| `long_description` |Description plus longue qui fournit davantage de détails sur le résultat.|
{: class="simple-tab-table"}
{: caption="Tableau 1. Description des composants généraux de la commande" caption-side="top"}
{: #definecard1}
{: tab-title="General"}
{: tab-group="card"}

| Signalé par | Description | 
|:-----------------|:-----------------|
| `id` | ID de l'outil de sécurité qui a signalé le résultat.  |
| `title` | Titre de l'outil de sécurité qui a signalé le résultat. |
{: class="simple-tab-table"}
{: caption="Tableau 2. Description des composants signalés de la commande" caption-side="top"}
{: #definecard2}
{: tab-title="Reported by"}
{: tab-group="card"}

| Carte | Description | 
|:-----------------|:-----------------|
| `section` | Section dans laquelle vous souhaitez que la carte s'affiche. Vous pouvez avoir jusqu'à trois sections personnalisées avec six cartes dans chaque section. Nombre maximal de caractères : 30  |
| Facultatif : `order` | Ordre dans lequel la carte est affichée au sein de la section spécifiée. L'ordre est spécifié dans une plage de 1 à 6. Si vous choisissez un numéro qui est déjà appliqué à une autre carte, la création échoue. Vous recevez le message d'erreur "Given order is already taken by other card in section." Si l'ordre fourni est supérieur au nombre de cartes actuelles plus 1, la création de carte échoue. Par exemple, si vous avez actuellement deux cartes et en créez une autre, vous ne pouvez pas spécifiez 5 dans l'ordre des cartes car vous n'avez que trois cartes au total. Si l'ordre de vos cartes n'est pas spécifié, elles sont classées par ordre alphabétique dans la section correspondante. |
| `title` | Titre que vous souhaitez donner à votre carte. Nombre maximal de caractères : 28  |
| `subtitle` | Sous-titre que vous souhaitez donner à votre carte. Nombre maximal de caractères : 30  |
| `finding_note_names` | `providers//notes/my-custom-tool-findings-type` |
{: class="simple-tab-table"}
{: caption="Tableau 3. Description des composants de carte de la commande" caption-side="top"}
{: #definecard3}
{: tab-title="Card"}
{: tab-group="card"}

| Eléments : Type de valeur | Description | 
|:-----------------|:-----------------|
| `kind` | Les options sont `NUMERIC`, `TIME_SERIES` et `BREAKDOWN`. |
| `text` | Texte que vous souhaitez afficher. Si 'kind' est défini sur `NUMERIC`, le nombre maximal de caractères est 60. Si 'kind' est défini sur `TIME_SERIES` ou `BREAKDOWN`, le nombre maximal de caractères est 65. |
| `default_time_range` | Période de temps à vérifier. Les valeurs sont définies en jours. Les options actuelles sont : `1d`, `2d`, `3d` et `4d`. |
| `value_type` | Type d'élément. Si 'kind' est défini sur `NUMERIC`, la zone est `value_type` et vous pouvez avoir jusqu'à quatre éléments par carte. Si 'kind' est défini sur `TIME_SERIES` ou `BREAKDOWN`, la zone est `value_types`. Le nombre maximal pour `TIME_SERIES` ou `BREAKDOWN` est 1. Si vous n'avez que des entrées numériques, vous pouvez avoir jusqu'à quatre éléments par carte. Si vous souhaitez utiliser une combinaison, vous pouvez avoir jusqu'à deux entrées numériques et une série temporelle ou une répartition. Vous ne pouvez pas avoir à la fois une série temporelle et une répartition dans la même carte. Si vous définissez vos types de valeur comme un tableau de séries temporelles, vous pouvez avoir jusqu'à trois entrées.  |
| `value_type` : `kind` | Type de valeur. Les options sont `KRI` et `FINDING_COUNT`. |
| `value_type` : `finding_note_names` | Si `kind` est défini sur `FINDING_COUNT` : nom du résultat que vous souhaitez voir dans votre carte, qui est spécifié en tant que tableau. |
| `value_type` : `kri_note_names` | Si `kind` est défini sur `FINDING_COUNT` : nom du résultat que vous souhaitez voir dans votre carte, qui est spécifié en tant que tableau. |
| `value_type` : `text` | Texte du type d'élément. Le nombre de caractères maximal est 22. |
{: class="simple-tab-table"}
{: caption="Tableau 4. Description des composants d'éléments de la commande" caption-side="top"}
{: #definecard4}
{: tab-title="Elements"}
{: tab-group="card"}

Exemple de réponse :

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
                        "url":"https://cloud.ibm.com/containers-kubernetes/clusters"
		}],
                "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
                "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod’s normal behavior"
	}
}
```
{: screen}
