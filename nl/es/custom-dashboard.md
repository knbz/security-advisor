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

# Integración de sus propias herramientas de seguridad
{: #custom}

{{site.data.keyword.security-advisor_long}} proporciona una API de hallazgos que se puede utilizar para unificar y mejorar la gestión de la seguridad en IBM Cloud. Puede utilizar las API de hallazgos para registrar nuevos tipos de hallazgos para los servicios asociados y herramientas de seguridad personalizadas. Una vez que se han registrado los metadatos, las herramientas pueden enviar cualquier aparición de los hallazgos como KPI y sucesos mediante las API. Los hallazgos se muestran como una tarjeta independiente en el panel de control de {{site.data.keyword.security-advisor_short}}.
{: shortdesc}

Las API de {{site.data.keyword.security-advisor_short}} API siguen la especificación de API de metadatos de artefactos de tipo [Grafeas](https://grafeas.io/) para almacenar, consultar y recuperar metadatos críticos correspondientes a los hallazgos notificados por todas las herramientas y servicios de seguridad.

## Integración de sus propias herramientas
{: #integrate}

**Antes de empezar**

1. Inicie sesión en {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud login
  ```
  {: codeblock}

2. Obtenga el ID de la cuenta. Asegúrese de que el ID tenga asignado el [rol de IAM](https://console.bluemix.net/iam/#/users) de **Gestor**. Para obtener más información sobre los roles de servicio, consulte las [políticas de acceso de {{site.data.keyword.security-advisor_short}}](/docs/services/security-advisor/iam.html).

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

3. Obtenga la señal de IAM. La señal se utiliza en la variable `--header` de cada solicitud de API.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

</br>

**Adición y supervisión de hallazgos**

1. Para registrar un nuevo tipo de hallazgo, cree una nota. Para crear la nota, utilice la [API de hallazgos](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote).

  Solicitud de ejemplo:

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Content-Type: application/json" -d "{ \"kind\": \"FINDING\", \"short_description\": \"My security tool finding\", \"long_description\": \"See what my custom security tool found\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-findings-type\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Custom Security Tool\" }, \"finding\": { \"severity\": \"MEDIUM\", \"next_steps\": [ { \"title\": \"Learn why this is reported as a risk\" } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icono de más información"/> Descripción de los componentes de este mandato </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>Una descripción breve que resume el hallazgo; no más de un par de palabras.</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>Una descripción más larga que contiene más información sobre el hallazgo.</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>La herramienta de seguridad personalizada.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>Un ID correspondiente al tipo de hallazgo que ha encontrado la herramienta de seguridad.</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>El ID de la herramienta de seguridad que ha notificado el hallazgo.</li><li>El título de la herramienta de seguridad que ha notificado el hallazgo.</li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>title</code></li></ul></td>
        <td></br><ul><li>El nivel de urgencia que presenta el hallazgo.</li> <li>Los pasos que se pueden seguir para solucionar el problema.</li> <li>El título del hallazgo.</li></ul></td>
      </tr>
    </tbody>
  </table>

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
  {: codeblock}

2. Para crear un hallazgo, publique una [aparición](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence).

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account-id>/providers/my-custom-tool/occurrences" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Replace-If-Exists: true" -H "Content-Type: application/json" -d "{ \"note_name\": \"<account-id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\", \"kind\": \"FINDING\", \"remediation\": \"how to resolve this\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-finding-1\", \"context\": { \"region\": \"location\", \"resource_id\": \"pluginId\", \"resource_name\": \"www.myapp.com\", \"resource_type\": \"worker\", \"service_name\": \"application\" }, \"finding\": { \"severity\": \"HIGH\", \"next_steps\": [{ \"url\": \"Details URL\" }] }}"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icono de más información"/> Descripción de los componentes de este mandato </th>
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
        <td>Los pasos que se deben seguir para solucionar el problema.</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>La herramienta de seguridad personalizada.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>Un ID correspondiente al tipo de hallazgo que ha encontrado la herramienta de seguridad.</td>
      </tr>
      <tr>
        <td><code>context</code><ul><li><code>región</code></li><li><code>resource_id</code></li> <li><code>resource_name</code></li> <li><code>resource_type</code></li> <li><code>service_name</code></li></ul></td>
        <td></br><ul><li><code>La ubicación en la que se ha producido el hallazgo</code></li><li><code>El ID correspondiente al recurso específico.</code></li> <li><code>El nombre del recurso. </code></li> <li><code>El tipo de recurso.</code></li> <li><code>El nombre del servicio.</code></li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>URL</code></li></ul></td>
        <td></br><ul><li>El nivel de urgencia que presenta el hallazgo.</li> <li>Los pasos que se pueden seguir para solucionar el problema.</li> <li>Un URL en el que se pueden encontrar los detalles del hallazgo.</li></ul></td>
      </tr>
    </tbody>
  </table>

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
  {: codeblock}

3. Defina la tarjeta en el panel de control para visualizar el hallazgo mediante la creación de una [nota](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote).

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam token>" -H "Content-Type: application/json" -d "{ \"kind\": \"CARD\", \"provider_id\": \"my-custom-tool\", \"id\": \"custom-tool-card\", \"short_description\": \"security risk found by my custom tool\", \"long_description\": \"Details about why this is security risk to be fixed\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Security Tool\" }, \"card\": { \"section\": \"My Security Tools\", \"title\": \"My Security Tool Findings\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ], \"elements\": [ { \"kind\": \"NUMERIC\", \"text\": \"Count of findings reported by my security tool\", \"default_time_range\": \"1d\", \"value_type\": { \"kind\": \"FINDING_COUNT\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ] } } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icono de más información"/> Descripción de los componentes de este mandato </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>CARD</code></td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>La herramienta de seguridad personalizada.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>Un ID correspondiente al tipo de hallazgo que ha encontrado la herramienta de seguridad.</td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>Una descripción, de no más de un par de palabras, que resume el hallazgo.</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>Una descripción más larga que contiene más información sobre el hallazgo.</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>El ID de la herramienta de seguridad que ha notificado el hallazgo.</li><li>El título de la herramienta de seguridad que ha notificado el hallazgo.</li></ul></td>
      </tr>
      <tr>
        <td><code>card</code> <ul><li><code>section</code></li> <li><code>title</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>La sección en la que tiene cabida la tarjeta.</li> <li>El título de la tarjeta</li> <li><code>providers/<provider_id>/notes/my-custom-tool-findings-type</code></li></ul></td>
      </tr>
      <tr>
        <td><code>elements</code> <ul><li><code>kind</code></li> <li><code>text</code></li> <li><code>default_time_range</code></li></ul></td>
        <td></br><ul><li>El tipo de elemento.</li> <li>El texto que desea mostrar</li> <li>El periodo de tiempo que desea comprobar.</li></ul></td>
      </tr>
      <tr>
        <td><code>value_type</code> <ul><li><code>kind</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>El tipo de valor</li> <li>El nombre de los hallazgos que desea ver en la tarjeta.</li></ul></td>
      </tr>
    </tbody>
  </table>

  Respuesta de ejemplo:
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

4. Vaya al panel de control del servicio para ver la tarjeta que ha creado.

## Caso de ejemplo
{: #example}

¿Por qué desea crear personalizaciones? Supongamos que tiene una aplicación que ejecuta un clúster de {{site.data.keyword.containershort_notm}} con llamado `cloudkingdom`. Uno de los pods del clúster está enviando una cantidad inusual de datos a servidores externos. Desea capturar este hallazgo en el panel de control de {{site.data.keyword.security-advisor_short}}.

Si la herramienta personalizada supervisa y detecta la cantidad inusual de datos que se están transfiriendo, la herramienta envía el hallazgo a {{site.data.keyword.security-advisor_short}}.

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
