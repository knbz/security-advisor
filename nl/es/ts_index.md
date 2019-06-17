---

copyright:
  years: 2019
lastupdated: "2019-06-06"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# Resolución de problemas
{: #troubleshooting}

Si tiene problemas mientras trabaja con {{site.data.keyword.security-advisor_long}}, considere estas técnicas para resolverlos y obtener ayuda.
{: shortdesc}


## Obtención de ayuda y soporte
{: #getting-help-and-support}



Para encontrar ayuda, busque en la información o plantee preguntas en el foro. También puede abrir una incidencia de soporte. Si utiliza el foro para hacer preguntas, etiquete su pregunta para que los equipos de desarrolladores de {{site.data.keyword.cloud_notm}} la puedan ver.
{: shortdesc}

  * Si tiene preguntas técnicas sobre {{site.data.keyword.security-advisor_short}}, publique la pregunta en [Stack Overflow](https://stackoverflow.com/){: external}. Asegúrese de incluir las etiquetas `security-advisor` e `ibm-cloud`.

  * Para formular preguntas sobre el servicio y obtener instrucciones de iniciación, utilice el foro [dW Answers](https://developer.ibm.com/){: external}. Asegúrese de incluir las etiquetas `security-advisor` e `ibm-cloud`.


Para obtener más información sobre cómo obtener ayuda, consulte [¿Cómo puedo obtener la ayuda que necesito?](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).


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
Antes de instalar una de las ofertas de Built-in Insights, debe instalar Helm. Para instalar Helm, puede utilizar la [documentación de integración del servicio Kubernetes](/docs/containers?topic=containers-helm).


## Problema conocido: No se puede crear una aparición de solución personalizada
{: #ts-custom-occurrence}

{: tsSymptoms}
Inténtelo de crear una aparición de solución personalizada, pero la información no se muestra en un navegador y no recibe ningún mensaje de error.

{: tsCauses}
No puede crear la aparición porque el nombre que ha elegido se está utilizando.

{: tsResolve}
Elija otro nombre para su aparición.

## Problema conocido: la imagen de conexión directa no se actualiza
{: #ts-known-cant-edit}

{: tsSymptoms}
Cuando carga o edita una imagen para una conexión directa, no se muestra inmediatamente.

{: tsCauses}
Es un problema conocido que las imágenes no se muestran correctamente.

{: tsResolve}
Para ver la imagen actualizada, renueve el explorador. La nueva imagen se mostrará en la tabla de conexión directa.

