---

copyright:
  years: 2019
lastupdated: "2019-02-18"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# Resolución de problemas
{: #troubleshooting}

Si tiene problemas mientras trabaja con {{site.data.keyword.security-advisor_long}}, considere estas técnicas para resolverlos y obtener ayuda.
{: shortdesc}


## Obtención de ayuda y soporte
{: #getting-help-and-support}



Para encontrar ayuda, busque en la información o plantee preguntas en el foro. También puede abrir una incidencia de soporte. Si utiliza el foro para hacer preguntas, etiquete su pregunta para que los equipos de desarrolladores de {{site.data.keyword.Bluemix_notm}} la puedan ver.
{: shortdesc}

* Si tiene preguntas técnicas sobre {{site.data.keyword.security-advisor_short}}, publique la pregunta en <a href="http://stackoverflow.com/" target="_blank">Stack Overflow <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. Asegúrese de incluir las etiquetas `security-advisor` e `ibm-cloud`.
* Para preguntas referentes al servicio e instrucciones sobre cómo empezar, utilice el foro <a href="https://developer.ibm.com/" target="_blank">dW Answers <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. Asegúrese de incluir las etiquetas `security-advisor` e `ibm-cloud`.

Para obtener más información sobre cómo obtener ayuda, consulte [¿Cómo puedo obtener la ayuda que necesito?](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).


## No se puede crear una aparición de solución personalizada
{: #ts-custom-occurrence}

{: tsSymptoms}
Inténtelo de crear una aparición de solución personalizada, pero la información no se muestra en un navegador y no recibe ningún mensaje de error.

{: tsCauses}
Ha encontrado un problema conocido. No puede crear la aparición porque el nombre que ha elegido ya existe.

{: tsResolve}
Elija otro nombre para su aparición.


## Error: el espacio de nombres "security-advisor-insights" está prohibido
{: #ts-error-helm-install}

{: tsSymptoms}
Cuando intenta instalar Network or Activity Insights, recibe el error:

```
El espacio de nombres “security-advisor-insights” está prohibido: El usuario “system:serviceaccount:kube-system:default” no puede obtener el recurso “namespaces” en el grupo de API “” en el espacio de nombres “security-advisor-insights”
```
{: screen}

{: tsCauses}
La cuenta de servicio predeterminada `kube-system` no tiene acceso de administración en el clúster.

{: tsResolve}
Antes de instalar una de las ofertas de Built-in Insights, debe instalar Helm. Para instalar Helm, puede utilizar la [documentación de integración del servicio Kubernetes](/docs/containers?topic=containers-integrations#helm).


## Defecto conocido: algunos hallazgos de Network Insights no se muestran
{: #ts-network-analytics}

{: tsSymptoms}
No ve todos los tipos de hallazgos cuando busca información de Network Insights a través del panel de control o la API.

{: tsCauses}
Algunos tipos de hallazgos de Network Insights solo funcionan en IBM Cloud Kubernetes Service versión 1.10 o anteriores.

{: tsResolve}
Utilice Kubernetes Service versión 1.10 o anterior.
