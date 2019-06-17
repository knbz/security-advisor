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


# Integraciones
{: #integrations}

Con {{site.data.keyword.security-advisor_long}}, puede integrar otras soluciones incorporadas y soluciones de socios o puede crear sus propias integraciones personalizadas.
{: shortdesc}


## Hallazgos preintegrados
{: #integrate-built-in}

Con {{site.data.keyword.security-advisor_long}}, puede obtener información de los problemas potenciales a través de las prestaciones de servicio incorporadas.
{: shortdesc}


De forma predeterminada, {{site.data.keyword.security-advisor_short}} se integra con:

* Certificate Manager para notificaciones relacionadas con la caducidad y el ciclo de vida de los certificados.
* Vulnerability Advisor para obtener detalles sobre vulnerabilidades y problemas de configuración.

Para obtener más información, consulte [Cómo aprovechar los servicios preintegrados](/docs/services/security-advisor?topic=security-advisor-setup-services).

</br>

## Integraciones de socios
{: #integrate-partner}

Las integraciones de socios son una forma de mejorar la seguridad de sus despliegues de {{site.data.keyword.cloud_notm}} mediante la creación de una conexión entre {{site.data.keyword.security-advisor_short}} y otras herramientas de seguridad que trabajan con IBM para garantizar una experiencia de usuario sin problemas.
{: shortdesc}

Entre los socios actuales de {{site.data.keyword.security-advisor_short}} se encuentran NeuVector y Twistlock.

¿Es usted un socio y está interesado en integrar su solución con {{site.data.keyword.security-advisor_short}}? Póngase en contacto con nuestro equipo enviando un mensaje a Matt Ward en wardm@us.ibm.com.
{: tip}

### NeuVector
{: #integrate-neuvector}

[NeuVector](https://neuvector.com){: external} proporciona una plataforma de seguridad automatizada y altamente integrada para Kubernetes y Red Hat OpenShift que le permite supervisar y proteger el tráfico de la red de contenedores. Específicamente, el tráfico de la red este-oeste.

Con NeuVector, puede detectar amenazas y violaciones de red para evitar ataques a las aplicaciones basadas en el contenedor. Mediante la supervisión, puede evitar las vulnerabilidades y fugas detectando escaladas de privilegios root, escaneados de puertos, shells inversos y actividad sospechosa de sistema de archivos en sus contenedores y hosts.

Para obtener ayuda para configurar la instancia de NeuVector, consulte el apartado sobre [Soluciones de socios](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-neuvector).


### Twistlock
{: #integrate-twistlock}

[Twistlock](https://www.twistlock.com){: external} permite prevenir riesgos a través de SDLC al impedir el despliegue de imágenes vulnerables en el entorno. La imposición de políticas automáticas y personalizadas permite controlar cada una de las fases del ciclo de vida de una aplicación. Twistlock Intelligence Stream detecta y agrega información sobre vulnerabilidades directamente de más de 30 proyectos, fuentes comerciales y resultados de investigaciones de los laboratorios de Twistlock.

Se centra en ofrecer los datos más precisos que cubran todos los niveles de la pila para poder presentar una visión precisa y la tasa más baja de falsos positivos.Twistlock combina estos datos con información procedente de sus despliegues reales, como por ejemplo qué contenedores están expuestos a internet, cuáles se ejecutan con un privilegio elevado y cuáles están sujetos a mitigaciones de seguridad, para que siempre pueda ver las vulnerabilidades más críticas en su entorno.

Para obtener ayuda para configurar la instancia de Twistlock, consulte el apartado sobre [Soluciones de socios](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-twistlock).
</br>


## Integraciones personalizadas
{: #integrate-custom}

Es posible que ya tenga una herramienta de seguridad de la que dependa. Puede integrar esta herramienta en el panel de control de {{site.data.keyword.security-advisor_short}} para que toda su información de seguridad esté centralizada en un solo lugar.
{: shortdesc}

### Creación de una conexión directa
{: #create-direct-connect}

Mediante el uso de la GUI de {{site.data.keyword.security-advisor_short}}, puede marcar las herramientas de seguridad internas y externas para tener un punto de acceso a todo desde el panel de control de {{site.data.keyword.security-advisor_short}}. Por ejemplo, puede marcar PagerDuty.

### Integración de sus herramientas con la API
{: #integrate-tools-api}

Con la API de hallazgos. puede integrar los hallazgos de las herramientas de seguridad personalizadas en el panel de control de {{site.data.keyword.security-advisor_short}}. Puede ser cualquier herramienta personalizada o de socio que genere una alerta de seguridad o un hallazgo que también admita la interacción basada en API.

## Perspectivas integradas
{: #integrate-insights}

Con las perspectivas integradas, puede detectar problemas potenciales mediante una supervisión continua de los registros del clúster y de la cuenta. Mediante la supervisión del tráfico de la red y de la actividad de los usuarios, puede ayudar a garantizar que los recursos de {{site.data.keyword.cloud_notm}} están siempre protegidos.
{: shortdesc}

### Network Insights (beta)
{: #integrate-network-insights}

Con Network Insights (beta), puede supervisar y analizar la comunicación de red del clúster, tanto de entrada como de salida, entre el clúster de Kubernetes y las entidades externas. Mediante el uso de la inteligencia de amenazas integrada y la detección de anomalías, el servicio puede identificar ataques de reconocimiento y activos cuya seguridad se vea potencialmente comprometida. Para obtener más información, consulte el apartado sobre [Network Insights](/docs/services/security-advisor?topic=security-advisor-network).

### Activity Insights (presentación)
{: #integrate-activity-insights}

Con Activity Insights (presentación), puede supervisar continuamente los registros de {{site.data.keyword.cloud_notm}} Activity Tracker para identificar la actividad no autorizada o sospechosa que realizan los usuarios o las apps mediante paquetes de reglas. Puede utilizar los paquetes de reglas que proporciona el servicio y que se basan en las prácticas recomendadas de seguridad o puede personalizar las reglas para que se ajusten a sus necesidades. Para obtener más información, consulte [Activity Insights](/docs/services/security-advisor?topic=security-advisor-activity).
