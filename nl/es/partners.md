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


# Socios
{: #setup-partner}

Las integraciones de IBM Business Partner constituyen un modo de incorporar alertas críticas y hallazgos de proveedores de terceros en el panel de control de {{site.data.keyword.security-advisor_long}}. Estos socios han trabajado con IBM para ayudar a configurar, simplificar e implantar la experiencia de integración del usuario.
{: shortdesc}

## Antes de empezar
{: #partner-before}

Antes de empezar a integrar socios, asegúrese de cumplir los siguientes requisitos previos.

* Asegúrese de que tiene una cuenta en el socio que desea integrar.
* Asegúrese de que dispone de los permisos administrativos necesarios para generar el URL de integración para el servicio de socio.
* Asegúrese de que tiene acceso de administración de IAM a {{site.data.keyword.cloud_notm}} y acceso de gestor a {{site.data.keyword.security-advisor_short}}.

## Asistente de integración
{: #wizard}

Como administrador con derechos administrativos tanto en la cuenta de {{site.data.keyword.cloud_notm}} como en la del socio, puede utilizar el asistente de integración para ponerse en marcha rápidamente. El asistente tiene cuatro pasos básicos:

* Establecer confianza y asociar las cuentas de {{site.data.keyword.cloud_notm}} y la del socio
* Copiar la información necesaria como, por ejemplo, las credenciales y los URL entre las cuentas
* Instalar los metadatos de hallazgos de socio en {{site.data.keyword.security-advisor_short}}
* Validar el emparejamiento publicando un hallazgo del socio en {{site.data.keyword.security-advisor_short}}


## Integración de NeuVector
{: #setup-neuvector}

Con NeuVector, puede detectar amenazas y violaciones de red para evitar ataques a las aplicaciones basadas en el contenedor. Mediante la supervisión, puede evitar las vulnerabilidades y fugas detectando escaladas de privilegios root, escaneados de puertos, shells inversos y actividad sospechosa de sistema de archivos en sus contenedores y hosts.
{: shortdesc}

Para integrar NeuVector con {{site.data.keyword.security-advisor_short}}, siga los pasos siguientes:

1. Vaya a **Integraciones > Integraciones de socio** y seleccione **NeuVector** entre las opciones que aparecen.
2. Conecte sus cuentas.
  1. Proporcione un nombre para la conexión.
  2. Proporcione el URL del panel de control de NeuVector.
  3. Proporcione el URL de configuración de NeuVector.
    1. Inicie sesión en NeuVector y vaya a la configuración.
    2. Pulse **Generar URL**.
    3. Copie el URL y péguelo en el asistente de integración de {{site.data.keyword.security-advisor_short}}.
  4. Pulse **Siguiente**.
3. Verifique que da permiso para que el servicio genere un ID de servicio y una clave de API y cree la conexión con el servicio pulsando **Siguiente**.
4. Pulse **Listo**.
5. Vaya al panel de control del servicio para revisar un hallazgo de prueba que NeuVector ha enviado a {{site.data.keyword.containershort_notm}}.



## Integración de Twistlock
{: #setup-twistlock}

Puede evitar riesgos deteniendo el despliegue de imágenes vulnerables en el entorno. Con la imposición de políticas automáticas y personalizadas, Twistlock ofrece un control completo de cada una de las fases del ciclo de vida de una aplicación.
{: shortdesc}

Cuando se configura la conexión de socio, se crean dos tarjetas en el panel de control que resumen los hallazgos de Twistlock.

**Twistlock Runtime** incorpora dos indicadores de riesgo clave (KRI):

* Número total de incidencias y auditorías: hallazgos relacionados con incidencias o auditorías en las cargas de trabajo nativas de la nube.
* Número total de auditorías de cortafuegos: hallazgos relacionados con problemas con el cortafuegos.

**Twistlock Vulnerabilities** incorpora un KRI:

* Número total de imágenes con vulnerabilidades: hallazgos relacionados con vulnerabilidades detectadas en las imágenes del contenedor.

Encontrará más información acerca de la compañía en la documentación de Twistlock.

### Configuración de Twistlock
{: #configure-twistlock}

1. En el panel de control de {{site.data.keyword.security-advisor_short}}, vaya a **Integraciones > Integraciones de socios** y seleccione **Twistlock** entre las opciones que se muestran.

2. Pulse **Sí, conectar mi cuenta a {{site.data.keyword.security-advisor_short}}**.

  Si aún no tiene una cuenta, pulse **No, ayudarme a crear una cuenta > Crear una cuenta**. Rellene la información y el equipo de ventas de Twistlock se pondrá en contacto con usted para ayudarle a comenzar.
  {: note}

3. Asigne un nombre a su conexión. Asegúrese de que el nombre sea exclusivo de la cuenta y de utilizar solo caracteres alfanuméricos, un espacio o un guion.

4. Opcional: si todavía no tiene un URL de configuración, pulse **Generar URL de registro** para ir a la documentación de Twistlock y aprender a crear el URL. Asegúrese de que tiene la señal de Twistlock que se entrega con la licencia para acceder a los documentos.

5. En el panel de control de Twistlock, vaya al separador **Gestionar > Alertas** y pulse **Añadir perfil**.

6. Seleccione **{{site.data.keyword.security-advisor_short}}** en el menú desplegable **Proveedor**.

7. Pulse **Copiar** para utilizar el URL de configuración proporcionado.

8. De nuevo en el panel de control de {{site.data.keyword.security-advisor_short}}, pegue el URL en el recuadro **Especificar URL de configuración de Twistlock**.

9. Pulse **Conectar socio**.

### Verificación de la conexión
{: #twistlock-verify}

1. En el panel de control de {{site.data.keyword.security-advisor_short}}, compruebe si las tarjetas Twistlock se muestran tal como se esperaba.

2. En el panel de control de Twistlock, renueve el separador **Alertas**. Se mostrará la conexión de {{site.data.keyword.security-advisor_short}}. Abra la conexión.

3. Compruebe que los tipos de alertas de los que desea ser notificado están marcados y pulse **Verificar** para asegurarse de que se ha completado la conexión.
