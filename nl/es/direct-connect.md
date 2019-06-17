---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

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


# Conexiones directas
{: #setup-custom-gui}

Con {{site.data.keyword.security-advisor_short}}, puede integrar las herramientas de seguridad personalizada existentes, ya sean de código abierto, de desarrollo personalizado o de servicios de terceros. Puede crear una conexión directa entre {{site.data.keyword.security-advisor_short}} y la otra herramienta añadiendo el URL a la lista de integraciones. Creando el enlace, puede supervisar fácilmente todas las herramientas que utiliza.
{: shortdesc}


## Antes de empezar
{: #custom-before-gui}

Para poder añadir la integración, debe tener una cuenta con el socio que desea integrar.

{{site.data.keyword.security-advisor_short}} no conserva las credenciales relacionadas con el servicio asociado. Los usuarios de empresa deben autenticarse mediante SAML tanto en {{site.data.keyword.cloud_notm}} como en el business partner.
{: note}

## Configuración de la conexión
{: #custom-configure-connection}

1. Inicie una sesión en la herramienta de seguridad y obtenga el URL exclusivo.

2. Inicie una sesión en {{site.data.keyword.cloud_notm}} mediante la consola.

3. Pulse **Integraciones personalizadas > Conexión directa**. Se mostrará una pantalla.

  1. Dé un nombre a la solución. En el nombre solo se pueden utilizar caracteres alfanuméricos, espacios en blanco y guiones (-).

  2. Especifique el URL para la solución con el formato: `www.<website>.<domain>`.

  3. Cargue un icono o una imagen para representar la herramienta.

  4. Pulse **Conectar** para completar la configuración. {{site.data.keyword.security-advisor_short}} crea los artefactos que son necesarios para la integración, como el ID de servicio, la clave de API, el ID de cuenta y los metadatos. Se asigna el rol de `escritor`.
