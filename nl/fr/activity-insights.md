---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

---

{:new_window: target="_blank"}
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


# Activity Insights (aperçu)
{: #activity}

Avec {{site.data.keyword.security-advisor_long}}, vous pouvez détecter toute activité d'utilisateur suspecte sur votre compte {{site.data.keyword.cloud_notm}} à l'aide d'{{site.data.keyword.cloud_notm}} Activity Tracker.
{: shortdesc}


## Fonctionnement
{: #activity-how}

La fonction Activity Insights est un module complémentaire  du service {{site.data.keyword.security-advisor_short}}. Lorsqu'elle est activée et configurée, le comportement utilisateur est journalisé et analysé afin d'identifier toute activité suspecte en fonction de règles. Vous pouvez utiliser des règles par défaut ou créer des règles personnalisées adaptées à votre organisation.

Reportez-vous à l'image ci-dessous pour examiner le flux d'informations.

![Organigramme d'Activity Insights](images/activity-insights-flow.png)

1. En tant qu'administrateur de compte, vous pouvez installer Activity Insights dans votre cluster.
2. Si le module complémentaire est installé dans un cluster, il peut surveiller les journaux Activity Tracker sur l'ensemble du compte.
3. Les journaux d'activité sont réacheminés dans un compartiment Cloud Object Storage, dans lequel ils sont stockés jusqu'à ce que vous décidiez de les supprimer. Lorsque vous utilisez l'interface graphique de {{site.data.keyword.security-advisor_short}} pour créer le compartiment, des rôles IAM service à service sont affectés pour que le service puisse voir les journaux.
4. Lorsqu'Activity Insights est activé, les données brutes qui se trouvent dans votre compartiment COS sont analysées en fonction de règles qui peuvent être prédéfinies par le service ou personnalisées par vous.
5. Lorsqu'un problème de sécurité potentiel est signalé, le résultat est réacheminé dans la base de données des résultats.
6. Les résultats sont affichés dans votre tableau de bord du service sur la carte **Activity Insights**.

</br>

## Collecte de données
{: #activity-data}

Activity Tracker collecte des événements qui décrivent les interactions utilisateur relatives aux API {{site.data.keyword.cloud_notm}}. Vous pouvez ensuite stocker les journaux dans un compartiment Object Storage pour une analyse approfondie.
{: shortdesc}

Activity Tracker collecte des événements qui décrivent les interactions utilisateur relatives aux API {{site.data.keyword.cloud_notm}}.

Les informations collectées incluent :

* L'adresse IP de l'initiateur de l'appel API
* L'utilisateur qui a été authentifié
* Le type d'activité
* Le résultat d'activité
* Etc...

Les données brutes qui sont collectées sont stockées dans un compartiment Cloud Object Storage dans lequel vous pouvez déterminer la durée de stockage. Vous possédez et contrôlez les données collectées, ce qui signifie que vous êtes en charge de leur stockage, de leur sécurisation et de leur suppression. {{site.data.keyword.security-advisor_short}} conserve les résultats pendant 90 jours. Au cours de cette période, les résultats sont présentés sur la carte **Activity Insights** dans le tableau de bord du service. Ainsi, même si vous ne voyez plus les résultats dans votre tableau de bord au bout de 90 jours, il se peut que les données brutes soient encore stockées.

Du point de vue de la sécurité, il est généralement recommandé de purger vos données collectées lorsque des obligations légales ou d'entreprise permettent leur suppression. Pour plus d'informations, voir [Suppression d'objets](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion).
{: tip}

## Activity Insights : accès
{: #ai-access}

La carte Activity Insights dans le tableau de bord du service récapitule toutes les indications d'activité de compte inattendue ou préoccupante de la part de vos utilisateurs et services. Une activité sortant de l'ordinaire peut correspondre à un comportement inapproprié de la part d'utilisateurs et de services légitimes, ou indiquer que votre compte est compromis. Les résultats figurant sur la carte sont déterminés en fonction de packages de règles par défaut fournis par {{site.data.keyword.security-advisor_short}}.

La carte comporte deux nouveaux indicateurs clé de risques :

* Identité et accès : résultats liés au service IAM (Identity and Access Management) ou au service {{site.data.keyword.appid_short_notm}}.
* Données et Kubernetes : résultats liés à Key Protect, Kubernetes Service, Cloud Object Storage ou Certificate Manager.


## Présentation des packages de règles
{: #activity-packages}

En tant qu'administrateur de compte, vous pouvez commencer rapidement à surveiller vos comptes en tirant parti des packages de règles.
{: shortdesc}

Le service propose des packages de règles qui sont associés à plusieurs services, notamment :

* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)
* {{site.data.keyword.cloudcerts_long_notm}}
* {{site.data.keyword.appid_long_notm}}
* {{site.data.keyword.keymanagementservicelong_notm}}
* {{site.data.keyword.cos_full_notm}} (COS)

Si les packages en cours ne répondent pas à tous vos besoins, vous pouvez mettre à jour un package existant ou créer un package pour adapter les règles à votre organisation.

### Qu'est-ce qu'une règle ?
{: #ai-rule}

Une règle est la combinaison de conditions et d'un événement unique. Vous pouvez utiliser des règles, ou une combinaison de règles, pour déclencher des résultats pouvant être affichés dans votre tableau de bord {{site.data.keyword.security-advisor_short}}.

Exemple :

```
{
	"comment": "Dormant Rule: Very high risk {{site.data.keyword.appid_short_notm}}  activity... ",
		"dormant": true,
		"conditions": { 	… },
		"event": { … }
	"type": "aggregate"
}
```
{: screen}

En plus d'une condition et d'un événement, les règles peuvent également contenir les zones qui se trouvent dans le tableau ci-dessous.

<table>
	<tr>
		<th colspan=2><img src="images/idea.png" alt="light bulb icon"/> Compréhension des composants d'une règle</th>
	</tr>
	<tr>
		<td><code>comment</code></td>
		<td>Toujours ignoré.</td>
	</tr>
	<tr>
		<td><code>dormant</code></td>
		<td>Zone booléenne qui, si la valeur est true, est ignorée. Si la valeur est false ou si elle n'est pas définie, la règle est utilisée.</td>
	</tr>
	<tr>
		<td><code>type</code></td>
		<td>Les options sont <code>aggregate</code>, <code>coincident</code> et <code>boolean</code>. Si le type n'est pas <code>aggregate</code> ou <code>coincident</code>, il est évalué comme étant <code>boolean</code>.</td>
	</tr>
</table>

</br>

### Qu'est-ce qu'une condition ?
{: #ai-condition}

Une condition de base est un bloc de construction constitué de trois composants. Les blocs sont liés avec les opérateurs `any` et `all` et peuvent être imbriqués. L'opérateur `all` est l'équivalent de l'opérateur `and` alors que l'opérateur `any` est l'équivalent de l'opérateur `or`.

Exemple :

```
"conditions": {
	"all": [{
		"any": [{
			"fact": "action",
				"operator": "equal",
				"value": "iam-groups.group.delete"
		},
		{
			"fact": "action",
				"operator": "equal",
				"value": "iam-groups.member.delete"
		}]
	}
}
```
{: screen}

<table>
	<tr>
		<th colspan=2><img src="images/idea.png" alt="light bulb icon"/> Compréhension des composants d'une condition</th>
	</tr>
	<tr>
		<td><code>fact</code></td>
		<td>Evénement Activity Tracker CADF qui est inspecté.</td>
	</tr>
	<tr>
		<td><code>operator</code></td>
		<td>Les options sont <code>equal</code>, <code>notEqual</code>, <code>lessThan</code>, <code>greaterThan</code>, <code>in</code> et <code>notIn</code>.</td>
	</tr>
	<tr>
		<td><code>value</code></td>
		<td>Définition d'une action. En général, la valeur correspond à un appel API qui peut être utilisé pour interagir avec le service.</td>
	</tr>
</table>

</br>

### Qu'est-ce qu'un événement ?
{: #ai-event}

Un événement se compose de deux zones : `type` et `params.findingType`. La première zone indique un identificateur unique pour une règle, alors que la zone `params.findingType` indique le nom du résultat qui est émis dans le service. Le nom du résultat permet d'afficher le résultat dans le tableau de bord {{site.data.keyword.security-advisor_short}}.

Exemple :

```
{
	"conditions": { 	… },
		"event": {
		"type": "IKS high risk API",
			"params": {"findingType": "IKS-high-risk"}
		}
}
```
{: screen}


### Type de règle : aggregate
{: #rule-aggregate}

Le type de règle aggregate compte le nombre d'occurrences d'une action dans une fenêtre de temps spécifique, puis déclenche un résultat si le seuil défini est dépassé. Vous définissez la règle en ajoutant le seuil et la fenêtre de temps à la condition booléenne. Plusieurs conditions doivent être remplies pour que la règle soit définie.

* Le type de règle doit être `aggregate`.
* La condition racine doit contenir les faits suivants :

	```
	{
			"fact": "occurrences",
			"operator": [equal | greaterThan | greaterThanInclusive],
			"value": [positive number]
	},
	{
	    "fact": "withInLast",
	    "operator": "equal",
	    "value": "X [minutes|hours]",
	}
	```
	{: screen}

	Quelques précisions :
	* X est égal à un entier positif différent de zéro
	* Lorsque la valeur hours est sélectionnée, la valeur maximale est 24
	* Lorsque la valeur minutes est sélectionnée, la valeur maximale est 1440

#### Exemple
{: #aggregate-example}

L'exemple suivant illustre une règle qui compte cinq tentatives ayant échoué au cours d'une période de 30 minutes :

```
{
    "conditions": {
        "all": [
            {
                "fact": "action",
                "operator": "equal",
                "value": "iam-identity.user-apikey.login"
            },
            {
                "fact": "reason",
                "operator": "equal",
                "value": 400,
                "path": ".reasonCode"
            },
            {
                "fact": "occurrences",
                "operator": "equal",
                "value": 5
            },
            {
                "fact": "withInLast",
                "operator": "equal",
                "value": "30 minutes",
            }
        ]
    },
    "event": {
        "type": "failed-login-attempts",
        "params": {
            "findingType": "failed-login-attempts",
        }
    },
    "type" : "aggregate"
}
```
{: screen}

### Type de règle : coincident
{: #rule-coincident}

Le type de règle coincident permet de surveiller les actions pour déterminer le nombre d'occurrences d'une même action au cours d'une fenêtre de temps spécifique. Vous définissez la règle en ajoutant une fenêtre de temps à un groupe de blocs de construction de condition de base. L'ordre dans lequel les actions ont lieu n'a pas de signification dans la règle coincident, mais plusieurs conditions doivent être remplies pour que la règle soit définie.

* Le type de règle doit être `coincident`.
* La condition racine doit être de la sorte `all`.
* La condition racine doit contenir les faits suivants :

	```
	{
	    "fact": "actions",
	    "operator": "contains",
	    "value": [action value]
	},
	{
	    "fact": "withInLast",
	    "operator": "equal",
	    "value": "X [minutes|hours]",
	}
	```
	{: screen}

	Quelques précisions :
	* La valeur `fact` doit être au pluriel : `actions`, et non `action`.
	* X est égal à un entier positif différent de zéro
	* Lorsque la valeur hours est sélectionnée, la valeur maximale est 24
	* Lorsque la valeur minutes est sélectionnée, la valeur maximale est 1440


#### Exemple
{: #coincident-example}

L'exemple suivant illustre une règle qui recherche une concomitance de trois actions spécifiques devant avoir lieu au cours d'une période de 30 minutes :

```
{
    "conditions": {
        "all": [
            {
                "fact": "actions",
                "operator": "contains",
                "value": "iam-identity.user-apikey.login"
            },
            {
                "fact": "actions",
                "operator": "contains",
                "value": "kms.secrets.list"
            },
            {
                "fact": "actions",
                "operator": "contains",
                "value": "kms.secrets.read"
            },
            {
                "fact": "withInLast",
                "operator": "equal",
                "value": "30 minutes",
            }
        ]
    },
    "event": {
        "type": "key-protect-keys-read",
        "params": {
            "findingType": "key-protect-keys-read",
        }
    },
    "type" : "coincident"
}
```
{: screen}

### Type de règle : boolean
{: #rule-boolean}

Une règle de type boolean se compose d'une condition booléenne et d'un événement. Les règles de type boolean sont souvent utilisées pour surveiller l'utilisation des API à risque élevé, l'utilisation des API en dehors de la fenêtre de contrôle des changements, ou l'utilisation des API par un initiateur qui ne figure pas sur liste blanche.

Si une règle n'est pas définie en tant que `aggregate` ou `coincident`, elle est évaluée comme étant de type `boolean`.

#### Exemple
{: #boolean-example}

L'exemple suivant illustre une règle qui recherche la suppression d'une stratégie hors de la fenêtre de contrôle des changements par un utilisateur qui ne figure pas sur la liste blanche :

```
{
	"conditions": {
		"all": [{
			"any": [{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.group.delete"
			},
			{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.member.delete"
			}]
		},
		{
			"any": [{
				"fact": "event_time",
				"operator": "lessThan",
				"value": "0800"
			},
			{
				"fact": "event_time",
				"operator": "greaterThan",
				"value": "1700"
			}
			]
		},
		{
			"fact": "initiator",
			"path": ".name",
			"operator": "notIn",
			"value": ["example@ibm.com", "example2@ibm.com"]
		}
		]
	},
	"event": {
		"type": "account-delete",
		"params": {
			"findingType": "iam-delete-account-threshold"
		}
	}
```
{: screen}

Vous voulez en savoir plus sur les règles booléennes ? Consultez la [documentation de Cache-Control](https://github.com/CacheControl/json-rules-engine/blob/master/docs/rules.md){: external}.
{: tip}

## Etapes suivantes
{: #activity-next}

Vous êtes prêt ? Lisez la rubrique relative à l'[activation d'Activity Insights](/docs/services/security-advisor?topic=security-advisor-setup-activity).
