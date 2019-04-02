---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-14"

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



# Sucesos de {{site.data.keyword.cloudaccesstrailshort}}
{: #at_events}

Puede ver, gestionar y auditar las actividades iniciadas por el usuario que se han realizado en la instancia del servicio {{site.data.keyword.security-advisor_long}} mediante el servicio {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

Para obtener más información sobre el funcionamiento del servicio, consulte la [documentación de {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker/reference?topic=cloud-activity-tracker-getting-started-with-cla#getting-started-with-cla).


## Dónde ver los sucesos
{: #monitor}

Los sucesos están disponibles en el **dominio de la cuenta** de {{site.data.keyword.cloudaccesstrailshort}} que está disponible en la región de {{site.data.keyword.Bluemix_notm}} donde se han generado los sucesos.

1. Inicie una sesión en su cuenta de {{site.data.keyword.Bluemix_notm}}.
2. Desde el catálogo, suministre una instancia del servicio {{site.data.keyword.cloudaccesstrailshort}} en la misma cuenta que la instancia de {{site.data.keyword.security-advisor_short}}.
3. En el separador **Gestión** del panel de control de {{site.data.keyword.cloudaccesstrailshort}}, pulse **Ver en Kibana**.
4. Establezca el intervalo de tiempo para el que desea ver los registros. El valor predeterminado es 15 minutos.
5. En la lista **Campos disponibles**, pulse **Tipo**. Pulse el icono de lupa de **Activity Tracker** para limitar los registros a aquellos que el servicio rastrea.
6. Puede utilizar los otros campos disponibles para limitar la búsqueda.

Para que los usuarios que no sean el propietario de la cuenta puedan ver los registros, debe utilizar el plan premium. Para permitir que otros usuarios vean sucesos, consulte [Concesión de permisos para ver sucesos de cuentas](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions).
{: tip}

## Lista de sucesos
{: #events}

Consulte la tabla siguiente para ver una lista de los sucesos que se envían a {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

<table>
  <tr>
    <th>Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>Ver una o varias notas creadas anteriormente.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>Crear una nota.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>Suprimir una nota.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>Actualizar una nota.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>Ver una o varias apariciones.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>Crear una aparición.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>Suprimir una aparición.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>Actualizar una aparición.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>Leer soluciones personalizadas que se han añadido al servicio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>Añadir una solución personalizada al servicio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>Actualizar una solución personalizada existente dentro del servicio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>Suprimir una solución personalizada del servicio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>Visualizar las soluciones de socio que ha añadido a la instancia de servicio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>Añadir una solución de socio a la instancia de servicio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>Actualizar una solución de socio en la instancia de servicio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>Suprimir una solución de socio de la instancia de servicio.</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.enable</code></td>
    <td>Habilitar la característica Network Insights que proporciona {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.disable</code></td>
    <td>Inhabilitar la característica Network Insights que proporciona {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.enable</code></td>
    <td>Habilitar la característica Activity Insights que proporciona {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.disable</code></td>
    <td>Inhabilitar la característica Activity Insights que proporciona {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code>security-advisor.insights-cos.create</code></td>
    <td>Crear una instancia de Cloud Object Storage mediante {{site.data.keyword.security-advisor_short}} para obtener información detallada de red y de actividad.</td>
  </tr>
</table>
