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


# Hallazgos personalizados
{: #setup_custom}

{{site.data.keyword.security-advisor_short}} le permite integrar las herramientas de seguridad personalizadas existentes, ya sean de código abierto, de desarrollo personalizado o de servicios de terceros. Integrando los hallazgos de terceros, puede reunir todas las herramientas de seguridad y los hallazgos en un panel de control, donde puede supervisar los sucesos de seguridad críticos.
{: shortdesc}


## Antes de empezar
{: #custom-before-api}

Para poder integrar hallazgos de la herramienta de terceros, asegúrese de cumplir los siguientes requisitos previos.

1. Asegúrese de que el ID de usuario o de servicio que utiliza tiene asignado el [rol de IAM](https://cloud.ibm.com/iam#/users) de **Gestor**.

2. Inicie sesión en {{site.data.keyword.cloud_notm}}.

  ```
  ibmcloud login
  ```
  {: codeblock}

3. Obtenga el ID de la cuenta. Para obtener más información sobre los roles de servicio, consulte las [políticas de acceso de {{site.data.keyword.security-advisor_short}}](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

4. Obtenga la señal de Identity and Access Management (IAM). La señal se utiliza en la variable `--header` de cada solicitud de API.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  Las señales de IAM caducan cada 60 minutos. Para obtener información sobre cómo [obtener
una nueva señal automáticamente](/docs/iam?topic=iam-iamtoken_from_apikey) mediante una clave de API.
  {: tip}



## Importación de hallazgos y de KRI
{: #custom-adding}

Las API siguen las especificaciones de metadatos de Grafeas como artefactos para almacenar, consultar y recuperar los metadatos críticos correspondientes a los hallazgos que notifican las herramientas de seguridad y los servicios. La integración puede hacerse utilizando las API para configurar Indicadores clave de riesgos (KRI).
{: shortdesc}


### Paso 1: Registrar un nuevo tipo de hallazgo
{: #custom-register-finding}

Para registrar un nuevo tipo de hallazgo, puede crear una nota. Para crear la nota, puede utilizar la [API de hallazgos](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}. Asegúrese de elegir un ID de proveedor exclusivo para identificar la herramienta personalizada. Si va a automatizar el proceso utilizando el ID de servicio como ID de proveedor.

Solicitud de ejemplo:

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{  \"kind\": \"FINDING\",  \"short_description\": \"My security tool finding\",  \"long_description\": \"Longer description of what the security tool found.\",  \"provder_id\": \"my-custom-tool\",  \"id\": \"my-custom-tool-findings-type\",  \"reported_by\": {    \"id\": \"my-custom-tool\",    \"title\": \"My custom security tool\"  } ,  \"finding\": {    \"severity\": \"MEDIUM\",    \"next_steps\": [      {        \"title\": \"Explain why it's reported as a risk.\"      }    ]  }}"
```
{: codeblock}

| General | Descripción | 
|:-----------------|:-----------------|
| `note_name` | `<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type`  |
| `kind` | `FINDING` |
| `remediation` | Los pasos que se deben seguir para solucionar el problema. |
| `provider_id` | La herramienta de seguridad personalizada. |
| `id` | Un ID correspondiente al tipo de hallazgo que ha encontrado la herramienta de seguridad. |
{: class="simple-tab-table"}
{: caption="Tabla 1. Componentes generales del mandato" caption-side="top"}
{: #registerfindingtable1}
{: tab-title="General"}
{: tab-group="register"}

| Contexto | Descripción | 
|:-----------------|:-----------------|
| `region` | La ubicación donde se ha producido el hallazgo.  |
| `resource_id` | El ID del recurso. |
| `resource_name` | El nombre del recurso. |
| `resource_type` | El tipo de recurso. |
| `service_name` | El nombre del servicio. |
{: class="simple-tab-table"}
{: caption="Tabla 2. Componentes de contexto del mandato" caption-side="top"}
{: #registerfinding2}
{: tab-title="Context"}
{: tab-group="register"}

| Hallazgo | Descripción | 
|:-----------------|:-----------------|
| `severity` | El nivel de urgencia que presenta el hallazgo.  |
| `next_steps` | Los pasos que se pueden seguir para solucionar el problema. |
| `url` | Un URL en el que se pueden encontrar los detalles del hallazgo. |
{: class="simple-tab-table"}
{: caption="Tabla 3. Componentes de hallazgo del mandato" caption-side="top"}
{: #registerfinding3}
{: tab-title="Finding"}
{: tab-group="register"}

Respuesta de ejemplo:

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

Asegúrese de recordar el nombre de la nota que se devuelve como parte de la respuesta. En este ejemplo, el valor es `/providers/my-custom-tool/notes/my-custom-tool-findings-type`. Este valor se utiliza en el siguiente paso.
{: tip}


### Paso 2: Publicar los hallazgos
{: #custom-post-findings}

Cree una [aparición](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence){: external} para publicar hallazgos como KRI o como sucesos en el panel de control del asesor de seguridad.

Para cada tarjeta, puede definir dos KRI.
{: note}

Solicitud de ejemplo: 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/occurrences"  -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"note_name\": \"<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\",    \"kind\": \"FINDING\",    \"remediation\": \"How to resolve Data leakage threat\",    \"provider_id\": \"my-custom-tool\",    \"id\": \"my-custom-tool-finding-2\",    \"context\": {        \"region\": \"location\",        \"resource_id\": \"cluster crn\",        \"resource_name\": \"cloudkingdom\",        \"resource_type\": \"container\",        \"service_name\": \"kubernetes service\"    },    \"finding\": {        \"severity\": \"HIGH\",        \"next_steps\": [{            \"title\": \"Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities\",                        \"url\":\"https://cloud.ibm.com/containers-kubernetes/clusters\"        }],                \"short_description\": \"One of the pods in your cluster appears to be leaking an excessive amount of data\",                \"long_description\": \"One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod's normal behavior\"    }}"
```
{: codeblock}

| General | Descripción | 
|:-----------------|:-----------------|
| `kind` | `FINDING` |
| `short_description` | Una descripción breve que resume el hallazgo; no más de un par de palabras. |
| `long_description` | Una descripción más larga que contiene información más detallada sobre el hallazgo. |
| `provider_id` | La herramienta de seguridad personalizada. |
| `id` | Un ID correspondiente al tipo de hallazgo que ha encontrado la herramienta de seguridad. |
{: class="simple-tab-table"}
{: caption="Tabla 2. Componentes generales del mandato" caption-side="top"}
{: #postfinding1}
{: tab-title="General"}
{: tab-group="post"}

| Notificado por | Descripción | 
|:-----------------|:-----------------|
| `id` | El ID de la herramienta de seguridad que ha notificado el hallazgo.  |
| `title` | El título de la herramienta de seguridad que ha notificado el hallazgo. |
{: class="simple-tab-table"}
{: caption="Tabla 2. Componentes de Notificado por del mandato" caption-side="top"}
{: #postfinding2}
{: tab-title="Reported by"}
{: tab-group="post"}

| Hallazgo | Descripción | 
|:-----------------|:-----------------|
| `severity` | El nivel de urgencia que presenta el hallazgo.  |
| `next_steps` | Los pasos que se pueden seguir para solucionar el problema. |
| `title` | El título del hallazgo. |
{: class="simple-tab-table"}
{: caption="Tabla 3. Componentes de hallazgo del mandato" caption-side="top"}
{: #postfinding3}
{: tab-title="Finding"}
{: tab-group="post"}


Respuesta de ejemplo:

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

### Paso 3: Definir la tarjeta a visualizar
{: #custom-define-card}

Define cómo desea que la búsqueda muestre los hallazgos en el panel de control creando una [note](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}.

Solicitud de ejemplo: 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"kind\": \"CARD\",        \"provider_id\": \"my-custom-tool\",    \"id\": \"custom-tool-card\",    \"short_description\": \"Security risk found by my custom tool\",        \"long_description\": \"More detailed description about why this security risk needs to be fixed\",    \"reported_by\": {      \"id\": \"my-custom-tool\",      \"title\": \"My security tool\"    },        \"card\": {      \"section\": \"My security tools\",      \"order\": 1 ,     \"title\": \"My security tool findings\",      \"subtitle\": \"My security tool\",      \"finding_note_names\": [        \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"      ],      \"elements\": [        {          \"kind\": \"NUMERIC\",          \"text\": \"Count of findings reported by my security tool\",          \"default_time_range\": \"1d\",          \"value_type\": {            \"kind\": \"FINDING_COUNT\",            \"finding_note_names\": [              \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"            ]          }        }      ]    }  }"
```
{: codeblock}

| General | Descripción | 
|:-----------------|:-----------------|
| `provider_id` | El ID de la herramienta de seguridad. |
| `id` | Un ID correspondiente al tipo de hallazgo que ha encontrado la herramienta de seguridad. |
| `short_description` | Una descripción breve que resume el hallazgo; no más de un par de palabras. |
| `long_description` | Una descripción más larga que contiene información más detallada sobre el hallazgo. |
{: class="simple-tab-table"}
{: caption="Tabla 1. Componentes generales del mandato" caption-side="top"}
{: #definecard1}
{: tab-title="General"}
{: tab-group="card"}

| Notificado por | Descripción | 
|:-----------------|:-----------------|
| `id` | El ID de la herramienta de seguridad que ha notificado el hallazgo.  |
| `title` | El título de la herramienta de seguridad que ha notificado el hallazgo. |
{: class="simple-tab-table"}
{: caption="Tabla 2. Componentes de Notificado por del mandato" caption-side="top"}
{: #definecard2}
{: tab-title="Reported by"}
{: tab-group="card"}

| Tarjeta | Descripción | 
|:-----------------|:-----------------|
| `section` | La sección donde desea que se muestre la tarjeta. Puede tener hasta seis secciones personalizadas con seis tarjetas en cada sección. Número máximo de caracteres: 30  |
| Opcional: `order` | El orden en que se muestra la tarjeta dentro de la sección especificada. El orden es un número del 1 al 6. Si elige un número que ya se ha especificado en otra tarjeta, falla la creación. Recibirá un mensaje de error: "El número de orden indicado ya está asignado a otra tarjeta de la sección". Si el número de orden indicado es mayor que el número de tarjetas actual más 1, falla la creación de la tarjeta. Por ejemplo, si actualmente tiene dos tarjetas y está creando otra, no puede especificar el 5 como número de orden de tarjeta porque en total tiene tres tarjetas. Si no se especifica el orden de ñas tarjetas, se ordenan alfabéticamente en la sección asignada. |
| `title` | El título que desea que tenga la tarjeta. Número máximo de caracteres: 28 |
| `subtitle` | El subtítulo que desea que tenga la tarjeta. Número máximo de caracteres: 30 |
| `finding_note_names` | `providers//notes/my-custom-tool-findings-type` |
{: class="simple-tab-table"}
{: caption="Tabla 3. Componentes de tarjeta del mandato" caption-side="top"}
{: #definecard3}
{: tab-title="Card"}
{: tab-group="card"}

| Elementos: Tipo de valor | Descripción | 
|:-----------------|:-----------------|
| `kind` | Las opciones incluyen: `NUMERIC`, `TIME_SERIES` y `BREAKDOWN`. |
| `text` | El texto que desea mostrar. Si el tipo es `NUMERIC`, el número máximo de caracteres es 60. Si el tipo es `TIME_SERIES` o `BREAKDOWN`, el número máximo de caracteres es 65. |
| `default_time_range` | El periodo de tiempo que desea comprobar. Los valores se establecen en días. Las opciones actuales incluyen: `1d`, `2d`, `3d` y `4d`. |
| `value_type` | El tipo de elemento. Si el tipo es `NUMERIC`, el campo es `value_type` y puede tener hasta cuatro elementos por tarjeta. Si el tipo es `TIME_SERIES` o `BREAKDOWN`, el campo es `value_types`. El número máximo tanto para `TIME_SERIES` como para `BREAKDOWN` es 1. Si sólo tiene entradas numéricas, puede tener hasta cuatro elementos por tarjeta. Si desea utilizar una combinación, puede tener hasta dos entradas numéricas y una de tipo serie temporal o desglose. No puede tener serie temporal y desglose en la misma tarjeta. Si define los tipos de valor como una matriz para las series temporales, puede tener hasta tres entradas.  |
| `value_type`: `kind` | El tipo de valor. Las opciones incluyen: `KRI` y `FINDING_COUNT`. |
| `value_type`: `finding_note_names` | Si `kind` es `FINDING_COUNT`, el nombre de los hallazgos que desea ver en la tarjeta, que está especificada como una matriz. |
| `value_type`: `kri_note_names` | Si `kind` es `FINDING_COUNT`, el nombre de los hallazgos que desea ver en la tarjeta, que está especificada como una matriz. |
| `value_type`: `text` | El texto del tipo de elemento. El número máximo de caracteres es 22. |
{: class="simple-tab-table"}
{: caption="Tabla 4. Componentes de elemento del mandato" caption-side="top"}
{: #definecard4}
{: tab-title="Elements"}
{: tab-group="card"}

Respuesta de ejemplo:

```
{
"author": {
  "account_id": "account id",
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


## Ejemplo de uso
{: #custom-example}

Supongamos que tiene una aplicación que se ejecuta en un clúster de {{site.data.keyword.containershort_notm}} con el nombre `cloudkingdom`. Dependiendo del tamaño de la aplicación, es posible que tenga varios pods en el clúster para poder supervisar todo al mismo tiempo. ¿Qué pasa si tiene varias herramientas personalizadas que supervisan y detectan en el clúster distintas amenazas? Si uno de los pods del clúster empieza a enviar una cantidad inusual de datos a servidores externos, querrá saberlo lo antes posible. La herramienta personalizada que supervisa la transferencia de datos puede detectar el hallazgo y enviarlo a {{site.data.keyword.security-advisor_short}}. Si tiene otra integración personalizada que detecta un problema, también enviaría el hallazgo a {{site.data.keyword.security-advisor_short}}. A continuación, {{site.data.keyword.security-advisor_short}} mostraría los hallazgos de todas las herramientas de supervisión en un solo panel de control. Allí puede obtener rápidamente una visión general de cualquier alerta, investigar un problema y obtener información sobre los pasos que debe emprender para solucionarlo.

Ejemplo de carga útil:

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
