---

copyright:
  years: 2014, 2018
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

# Configuración de {{site.data.keyword.security-advisor_short}}
{: #setup}

Con {{site.data.keyword.security-advisor_long}}, puede supervisar continuamente las apps para detectar riesgos de seguridad y amenazas. Cuando se detectan vulnerabilidades, se le alerta a través del panel de control del servicio.
{:shortdesc}

## Supervisión de vulnerabilidades en imágenes de contenedor
{: #setup_images}

Una imagen de Docker es la base de cada contenedor que se crea. La imagen puede contener la app, su configuración y las dependencias necesarias. Una imagen se suele almacenar en un registro. Mediante {{site.data.keyword.registryshort_notm}} puede acceder a Vulnerability Advisor, que explora continuamente las imágenes en busca de problemas de seguridad potenciales. Si se encuentran problemas, se le alerta y puede ver un informe completo en el panel de control de {{site.data.keyword.security-advisor_short}}.
{:shortdesc}

Obtenga más información acerca de [{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html#index).


**Antes de empezar**

Antes de empezar a trabajar con el registro, asegúrese de que tiene instaladas las siguientes CLI y plugins:
- [La CLI de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://clis.ng.bluemix.net/ui/home.html)
- El plugin container-registry.

    ```
    ibmcloud plugin install container-registry -r Bluemix
    ```
    {: pre}

</br>
**Creación de un espacio de nombres**

1. Inicie una sesión en su cuenta mediante la CLI.

   ```
   bx login --sso
   ```
   {: pre}

2. Inicie sesión en {{site.data.keyword.registryshort_notm}}.

   ```
   bx cr login
   ```
   {: pre}

3. Opcional: cree un espacio de nombres. Siempre puede utilizar uno existente.

   ```
   bx cr namespace-add
   ```
   {: pre}

3. Etiquete una imagen.

   ```
   docker tag <image>:<tag> registry.ng.bluemix.net/<namespace>/<image>:<tag>
   ```
   {: pre}

5. Envíe la imagen por push.

   ```
   docker push registry.ng.bluemix.net/<namespace>/<image>:<tag>
   ```
   {: pre}


Después de enviar por push imágenes al espacio de nombres de {{site.data.keyword.registryshort_notm}}, se muestra información sobre las vulnerabilidades encontradas en la tarjeta **Imágenes con vulnerabilidades** en el panel de control del servicio. También puede ver detalles de imágenes específicas para encontrar más información, como por ejemplo la descripción de todas las vulnerabilidades conocidas y los problemas de configuración que se han identificado.

</br>

## Supervisión de certificados
{: #setup_certificates}

¿Sabía que {{site.data.keyword.cloudcerts_long_notm}} puede ayudar a supervisar y gestionar los certificados SSL/TLS? Mediante la integración de {{site.data.keyword.cloudcerts_short}} y {{site.data.keyword.security-advisor_short}}, puede recibir alertas y recordatorios sobre cuándo debería actualizar los certificados y otra información, lo que puede ayudar a evitar futuras vulnerabilidades.
{:shortdesc}

Puede obtener más información sobre [{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager/index.html#gettingstarted).

1. En el catálogo de {{site.data.keyword.Bluemix_notm}}, busque "{{site.data.keyword.cloudcerts_short}}".
2. Dé un nombre a la instancia de servicio, o utilice el nombre preestablecido.
3. Pulse **Crear**.
4. Para importar los certificados de la organización en {{site.data.keyword.cloudcerts_short}}, pulse **Importar certificado**.

Ahora que se han importado los certificados, se muestra información, como tiempos de caducidad y certificados caducados, en la tarjeta **Certificados** en el panel de control de {{site.data.keyword.security-advisor_short}}. Si pulsa en la tarjeta, obtendrá información más específica sobre los certificados, como por ejemplo la instancia de servicio a la que pertenecen los certificados. También puede ver los pasos que puede llevar a cabo para solucionar las vulnerabilidades de seguridad.
