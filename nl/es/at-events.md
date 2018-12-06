---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-11"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Sucesos de {{site.data.keyword.cloudaccesstrailshort}}
{: #at_events}

Puede ver, gestionar y auditar las actividades iniciadas por el usuario que se han realizado en la instancia del servicio {{site.data.keyword.security-advisor_long}} mediante el servicio {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

Para obtener más información sobre el funcionamiento del servicio, consulte la [documentación de {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker/index.html).


## Dónde ver los sucesos
{: #monitor}

Los sucesos están disponibles en el **dominio de la cuenta** de {{site.data.keyword.cloudaccesstrailshort}} que está disponible en la región de {{site.data.keyword.Bluemix_notm}} donde se han generado los sucesos.

1. Inicie sesión en su cuenta de {{site.data.keyword.Bluemix_notm}}.
2. Desde el catálogo, suministre una instancia del servicio {{site.data.keyword.cloudaccesstrailshort}} en la misma cuenta que la instancia de {{site.data.keyword.security-advisor_long}}.
3. En el separador **Gestión** del panel de control de {{site.data.keyword.cloudaccesstrailshort}}, pulse **Ver en Kibana**.
4. Establezca el intervalo de tiempo para el que desea ver los registros. El valor predeterminado es 15 minutos.
5. En la lista **Campos disponibles**, pulse **Tipo**. Pulse el icono de lupa de **Activity Tracker** para limitar los registros a aquellos que el servicio rastrea.
6. Puede utilizar los otros campos disponibles para limitar la búsqueda.

Para que los usuarios que no sean el propietario de la cuenta puedan ver los registros, debe utilizar el plan premium. Para permitir que otros usuarios vean sucesos, consulte [Concesión de permisos para ver sucesos de cuentas](/docs/services/cloud-activity-tracker/how-to/grant_permissions.html#grant_permissions).
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
    <td>Suprima una aparición.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>Actualizar una aparición.</td>
  </tr>
</table>
