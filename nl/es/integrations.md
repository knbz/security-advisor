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

# Integraciones
{: #about}

Con {{site.data.keyword.security-advisor_long}}, puede integrar los hallazgos de IBM Cloud Services, las soluciones de socios, o crear sus propias integraciones personalizadas.
{: shortdesc}


## Hallazgos preintegrados
{: #built-in}

Con {{site.data.keyword.security-advisor_long}}, puede obtener información de los problemas potenciales a través de las prestaciones de servicio incorporadas.
{: shortdesc}


De forma predeterminada, {{site.data.keyword.security-advisor_short}} se integra con:

* Certificate Manager para notificaciones relacionadas con la caducidad y el ciclo de vida de los certificados.
* Vulnerability Advisor para obtener detalles sobre vulnerabilidades y problemas de configuración.

Para obtener más información, consulte [Cómo aprovechar los servicios preintegrados](setup.html).

</br>

## Soluciones de socios
{: #partner}

Las soluciones de socios son una forma de mejorar la seguridad de los despliegues de IBM Cloud creando un enlace entre {{site.data.keyword.security-advisor_short}} y otras herramientas de seguridad fuera de IBM Cloud.
{: shortdesc}

{{site.data.keyword.security-advisor_short}} está asociado actualmente con las siguientes empresas:

### NeuVector
{: #neuvector}

[ NeuVector ](https://neuvector.com/) proporciona una plataforma de seguridad automatizada y altamente integrada para Kubernetes y Red Hat OpenShift que le permite supervisar y proteger el tráfico de la red de contenedores. Específicamente, el tráfico de la red este-oeste.

Con NeuVector, puede detectar amenazas y violaciones de red para evitar ataques a las aplicaciones basadas en el contenedor. Mediante la supervisión, puede evitar las vulnerabilidades y fugas detectando escaladas de privilegios root, escaneados de puertos, shells inversos y actividad sospechosa de sistema de archivos en sus contenedores y hosts.

Para obtener más información sobre cómo configurar la instancia de NeuVector con el asistente de integración, consulte [Soluciones de socios](partners.html).

¿Es usted un socio y está interesado en integrar su solución con {{site.data.keyword.security-advisor_short}}? Póngase en contacto con nuestro equipo enviando un mensaje a Matt Ward en wardm@us.ibm.com.
{: tip}

</br>

## Integraciones personalizadas
{: #custom}

Es posible que ya tenga una herramienta de seguridad de la que dependa. Puede integrar esta herramienta en el panel de control de {{site.data.keyword.security-advisor_short}} para que toda su información de seguridad esté centralizada en un solo lugar.
{: shortdesc}

**Integración de sus herramientas con la GUI**

Mediante el uso de la GUI de {{site.data.keyword.security-advisor_short}}, puede marcar las herramientas de seguridad internas y externas para tener un punto de acceso a todo desde el panel de control de {{site.data.keyword.security-advisor_short}}. Por ejemplo, puede marcar PagerDuty.

**Integración de sus herramientas con la API**

Con la API de hallazgos. puede integrar los hallazgos de las herramientas de seguridad personalizadas en el panel de control de {{site.data.keyword.security-advisor_short}}. Puede ser cualquier herramienta personalizada o de socio que genere una alerta de seguridad o un hallazgo que también admita la interacción basada en API.

**Cómo empezar**

Para conocer las prácticas recomendadas sobre cómo aprovechar las API y crear marcadores a través de la GUI, consulte [Herramientas de seguridad personalizadas](/docs/services/security-advisor/custom.html).

</br>


## Network Analytics (vista previa)
{: #network-analytics}

Con la vista previa de Network Analytics, puede detectar amenazas supervisando la comunicación de clúster de {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Mediante la supervisión de la comunicación de clúster para la comunicación sospechosa de y hacia los clústeres, puede aprovechar la información de amenazas que ofrece IBM X-Force para señalar cualquier problema potencial. Cuando se identifica una comunicación sospechosa, se emite un resultado que puede revisarse y evaluarse.

Para obtener información más detallada sobre Network Analytics, consulte [Network Analytics](network-analytics.html).
{: tip}

</br>
</br>
