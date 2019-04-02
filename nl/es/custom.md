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


# Personalización
{: #setup_custom}

{{site.data.keyword.security-advisor_short}} le permite integrar las herramientas de seguridad personalizadas existentes, ya sean de código abierto, de desarrollo personalizado o de servicios de terceros. Puede crear una conexión directa entre {{site.data.keyword.security-advisor_short}} y la otra herramienta añadiendo el URL a la lista de integraciones. También puede integrar los hallazgos de terceros para incorporar sucesos de seguridad críticos directamente en una nueva tarjeta en el panel de control utilizando las API para configurar las API y los indicadores clave de riesgo (KRI).
{: shortdesc}


## Adición de una conexión directa
{: #setup-custom-gui}

Puede realizar fácilmente un seguimiento de las herramientas de seguridad utilizando el panel de control de {{site.data.keyword.security-advisor_short}}.
{: shortdesc}

### Antes de empezar
{: #custom-before-gui}

Para poder añadir la integración, debe tener una cuenta con el socio que desea integrar.

{{site.data.keyword.security-advisor_short}} no conserva las credenciales relacionadas con el servicio asociado. Los usuarios de empresa deben autenticarse mediante SAML tanto en {{site.data.keyword.Bluemix_notm}} como en el business partner.
{: note}

### Configuración de la conexión
{: #custom-configure-connection}

1. Inicie una sesión en la herramienta de seguridad y obtenga el URL exclusivo.

2. Inicie una sesión en {{site.data.keyword.cloud_notm}} mediante la consola.

3. Pulse **Integraciones personalizadas > Conexión directa**. Se mostrará una pantalla.

  1. Dé un nombre a la solución. En el nombre solo se pueden utilizar caracteres alfanuméricos, espacios en blanco y guiones (-).

  2. Especifique el URL para la solución con el formato: `www.<sitio_web>.<dominio>`.

  3. Cargue un icono o una imagen para representar la herramienta.

  4. Pulse **Conectar** para completar la configuración. {{site.data.keyword.security-advisor_short}} crea los artefactos que son necesarios para la integración, como el ID de servicio, la clave de API, el ID de cuenta y los metadatos. Se asigna el rol de `escritor`.



## Integración de hallazgos de terceros

Las API siguen las especificaciones de metadatos de Grafeas como artefactos para almacenar, consultar y recuperar los metadatos críticos correspondientes a los hallazgos que notifican las herramientas de seguridad y los servicios.
{: #setup-custom-api}

### Antes de empezar
{: #custom-before-api}

Para poder integrar hallazgos de la herramienta de terceros, asegúrese de cumplir los siguientes requisitos previos.

1. Asegúrese de que el ID de usuario o de servicio que utiliza tiene asignado el [rol de IAM](https://cloud.ibm.com/iam#/users) de **Gestor**.

1. Inicie sesión en {{site.data.keyword.cloud_notm}}.

  ```
  ibmcloud login
  ```
  {: codeblock}

2. Obtenga el ID de la cuenta. Para obtener más información sobre los roles de servicio, consulte las [políticas de acceso de {{site.data.keyword.security-advisor_short}}](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

3. Obtenga la señal de Identity and Access Management (IAM). La señal se utiliza en la variable `--header` de cada solicitud de API.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  Las señales de IAM caducan cada 60 minutos. Para obtener información sobre cómo [obtener
una nueva señal automáticamente](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey) mediante una clave de API.
  {: tip}



### Importación de hallazgos y de KRI
{: #custom-adding}

1. Para registrar un nuevo tipo de hallazgo, cree una nota. Para crear la nota, utilice la [API de hallazgos](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote). Asegúrese de elegir un ID de proveedor exclusivo para identificar la herramienta personalizada. Si va a automatizar el proceso mediante un ID de servicio, puede utilizar el ID de servicio como ID de proveedor.

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

2. Publique los hallazgos como KRI o como sucesos, conocidos también como [apariciones](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence).

  Para cada tarjeta, puede definir dos KRI.
  {: note}

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
  {: screen}

4. Vaya al panel de control del servicio para ver la tarjeta que ha creado.

## Ejemplo de uso
{: #custom-example}

Supongamos que tiene una aplicación que se ejecuta en un clúster de {{site.data.keyword.containershort_notm}} con el nombre `cloudkingdom`. Dependiendo del tamaño de la aplicación, es posible que tenga varios pods en el clúster para poder supervisar todo al mismo tiempo. ¿Qué pasa si tiene varias herramientas personalizadas que supervisan y detectan el clúster en busca de distintas amenazas? Si uno de los pods del clúster empieza a enviar una cantidad inusual de datos a servidores externos, querrá saberlo lo antes posible. La herramienta personalizada que supervisa la transferencia de datos puede detectar el hallazgo y enviarlo a {{site.data.keyword.security-advisor_short}}. Si tiene otra integración personalizada que detecta un problema, también enviaría el hallazgo a {{site.data.keyword.security-advisor_short}}. A continuación, {{site.data.keyword.security-advisor_short}} mostraría los hallazgos de todas las herramientas de supervisión en un solo panel de control. Allí puede obtener rápidamente una visión general de cualquier alerta, investigar un problema y obtener información sobre los pasos que debe emprender para solucionarlo.


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
                        "url":"https://console.bluemix.net/containers-kubernetes/clusters"
		}],
                "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
                "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod’s normal behavior"
	}
}
```
{: screen}
