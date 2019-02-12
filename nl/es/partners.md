---

copyright:
  years: 2018
lastupdated: "2018-12-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Soluciones de socios
{: #setup-partner}

Las soluciones de socios son una forma de ampliar la seguridad creando un enlace entre {{site.data.keyword.security-advisor_long}} y otras herramientas de seguridad.
{: shortdesc}

## Asistente de integración
{: #wizard}

Como administrador con derechos administrativos tanto en las cuentas de IBM Cloud y de socio, puede utilizar el asistente de integración para ponerse en marcha rápidamente. El asistente tiene cuatro pasos básicos:

* Establecer la confianza y asociar las cuentas de IBM Cloud y de socio
* Copiar la información necesaria como, por ejemplo, las credenciales y los URL entre las cuentas
* Instalar los metadatos de hallazgos de socio en {{site.data.keyword.security-advisor_short}}
* Validar el emparejamiento publicando un hallazgo del socio en {{site.data.keyword.security-advisor_short}}

</br>

## Integración de NeuVector
{: #neuvector}

**Antes de empezar**

* Asegúrese de que tiene una cuenta en el socio que desea integrar.
* Asegúrese de que dispone de los permisos administrativos necesarios para generar el URL de integración para el servicio de socio.
* Asegúrese de que tiene acceso de administración de IAM a IBM Cloud y acceso de gestor a {{site.data.keyword.security-advisor_short}}.

**Integración**

1. Conecte sus cuentas.
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
