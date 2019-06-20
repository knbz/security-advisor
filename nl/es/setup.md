---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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

# Cómo aprovechar los servicios preintegrados
{: #setup-services}

{{site.data.keyword.security-advisor_short}} se suministra con varias tarjetas rellenadas previamente que pueden ayudarle a supervisar los riesgos y amenazas de seguridad.
{: shortdesc}

{{site.data.keyword.security-advisor_short}} crea automáticamente una tarjeta para los siguientes servicios:

* {{site.data.keyword.registrylong_notm}}
* {{site.data.keyword.cloudcerts_long_notm}}

Aunque no tiene que hacer nada para crear la conexión entre {{site.data.keyword.security-advisor_short}} y los servicios, debe tener instancias de los servicios configuradas con información.


## Supervisión de vulnerabilidades en imágenes de contenedor
{: #setup-images}

Con {{site.data.keyword.registryshort_notm}}, tiene acceso a Vulnerability Advisor, que explora de forma continuada las imágenes de su instancia de {{site.data.keyword.registryshort_notm}} en busca de problemas de seguridad potenciales. Si se encuentran problemas, se le alerta y puede ver un informe completo en el panel de control de {{site.data.keyword.security-advisor_short}}.
{:shortdesc}

Obtenga más información acerca de [{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started).


### Antes de empezar
{: #setup-before}

Para poder empezar a trabajar con el registro, asegúrese de que tiene instaladas las siguientes CLI y plugins:
* [La CLI de {{site.data.keyword.cloud_notm}})](/docs/cli?topic=cloud-cli-ibmcloud-cli)
* El plugin Container Registry.

  ```
  ibmcloud plugin install container-registry
  ```
  {: codeblock}


### Creación de un espacio de nombres
{: #setup-create-namespace}

1. Inicie una sesión en su cuenta mediante la CLI.

  ```
  ibmcloud login --sso
  ```
  {: codeblock}

2. Inicie sesión en {{site.data.keyword.registryshort_notm}}.

  ```
  ibmcloud cr login
  ```
  {: codeblock}

3. Opcional: cree un espacio de nombres. Siempre puede utilizar uno existente.

  ```
  ibmcloud cr namespace-add
  ```
  {: codeblock}

3. Etiquete una imagen.

  ```
  docker tag <image>:<tag> <region>.icr.io/<namespace>/<image>:<tag>
  ```
  {: codeblock}

5. Envíe la imagen por push.

  ```
  docker push <region>.icr.io/<namespace>/<image>:<tag>
  ```
  {: codeblock}


Después de enviar por push imágenes al espacio de nombres de {{site.data.keyword.registryshort_notm}}, se muestra información sobre las vulnerabilidades encontradas en la tarjeta **Imágenes con vulnerabilidades** en el panel de control del servicio. También puede ver detalles de imágenes específicas para encontrar más información, como por ejemplo una descripción de todas las vulnerabilidades y problemas de configuración que se han identificado.


## Supervisión de certificados
{: #setup-certificates}

¿Sabía que {{site.data.keyword.cloudcerts_long_notm}} puede ayudar a supervisar y gestionar los certificados SSL/TLS? Mediante la integración de {{site.data.keyword.cloudcerts_short}} y {{site.data.keyword.security-advisor_short}}, puede obtener alertas por adelantado sobre la caducidad de los certificados, lo que puede ayudar a evitar la interrupción de un servicio o aplicación.
{:shortdesc}

En función de la fecha de caducidad del certificado que cargue a {{site.data.keyword.cloudcerts_short}}, los hallazgos aparecerán en el panel de control de {{site.data.keyword.security-advisor_short}} 90, 60, 10 y 1 días antes de que caduque el certificado. Además, recibirá notificaciones diarias sobre los certificados caducados. Las notificaciones diarias empiezan el día después de que caduque el certificado.

Para desencadenar una actualización manual, puede intentar cargar un certificado que caduque en un día a su instancia de {{site.data.keyword.cloudcerts_short}}. Cuando la importación se haya realizado correctamente, puede ver que el indicador clave de riesgo (KRI) y los resultados son visibles en el panel de control de {{site.data.keyword.security-advisor_short}}.

Encontrará más información acerca de [{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager?topic=certificate-manager-getting-started) en la documentación.
{: tip}

### Creación de un certificado
{: #setup-create-cert}

Para crear un certificado autofirmado que caduque en un día, puede ejecutar el siguiente mandato OpenSSL en el terminal.

```
openssl req -x509 -newkey rsa:2048 -keyout key.pem -subj "/CN=myservice.com" -out server.pem -days 1 -nodes
```
{: codeblock}


### Cargar un certificado
{: #setup-upload-cert}

1. En el catálogo de {{site.data.keyword.cloud_notm}}, busque "{{site.data.keyword.cloudcerts_short}}".
2. Dé un nombre a la instancia de servicio, o utilice el nombre preestablecido.
3. Pulse **Crear**.
4. Para importar los certificados de la organización en {{site.data.keyword.cloudcerts_short}}, pulse **Importar certificado**.

Ahora que se han importado los certificados, se muestra información, como tiempos de caducidad y certificados caducados, en la tarjeta **Certificados** en el panel de control de {{site.data.keyword.security-advisor_short}}. Si pulsa en la tarjeta, obtendrá información más específica sobre los certificados, como por ejemplo la instancia de servicio a la que pertenecen los certificados. También puede ver los pasos que puede llevar a cabo para solucionar las vulnerabilidades de seguridad.

Para detener las notificaciones, debe renovar el certificado, cargar el certificado en {{site.data.keyword.cloudcerts_short}} y suprimir el certificado caducado.
{: tip}
