---

copyright:
  years: 2017, 2018
lastupdated: "2018-12-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Gestión de acceso de servicio
{: #service-access}

Como propietario de la cuenta, puede gestionar el acceso a las instancias de {{site.data.keyword.security-advisor_long}} mediante {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). Mediante el establecimiento de políticas dentro de su cuenta que crean distintos niveles de acceso para los distintos usuarios, puede asegurarse de que cada instancia de {{site.data.keyword.security-advisor_short}} esté protegida.
{: shortdesc}

Para obtener más información sobre IAM, consulte [Acceso de IAM](/docs/iam/users_roles.html).

## Políticas de acceso de {{site.data.keyword.security-advisor_short}}
{: #access}

Todos los usuarios que acceden a una instancia del servicio {{site.data.keyword.security-advisor_short}} en su cuenta deben tener asignada una política de acceso con un rol de usuario IAM definido. La política determina qué acciones puede realizar un usuario dentro del contexto de la instancia deservicio específica.
{: shortdesc}

Las acciones son personalizadas y están definidas por el servicio de {{site.data.keyword.Bluemix_notm}} como operaciones permitidas para realizarse en el servicio. Las acciones se correlacionarán entonces a los roles de usuario del servicio IAM. En la tabla siguiente, se correlacionan las acciones y los permisos necesarios para {{site.data.keyword.security-advisor_short}}.

<table><caption>¿Qué roles puede realizar cada acción?</caption>
  <col width="40%">
  <col width="40%">
  <col width="20%">
  <tr>
    <th>Acción</th>
    <th>Explicación</th>
    <th>Rol</th>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>Ver hallazgos del informe de seguridad.</td>
    <td>Lector</br>Escritor</br>Gestor</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>Genere hallazgos del informe de seguridad.</td>
    <td>Escritor</br>Gestor</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>Suprimir hallazgos del informe de seguridad.</td>
    <td>Gestor</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>Editar hallazgos del informe de seguridad.</td>
    <td>Escritor</br>Gestor</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>Ver metadatos.</td>
    <td>Lector</br>Escritor</br>Gestor</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>Suprimir metadatos.</td>
    <td>Gestor</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>Generar metadatos.</td>
    <td>Gestor</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>Actualizar metadatos.</td>
    <td>Gestor</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>Leer soluciones personalizadas que se han añadido al servicio.</td>
    <td>Lector</br>Escritor</br>Gestor</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>Añada una solución personalizada al servicio.</td>
    <td>Gestor</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>Actualizar una solución personalizada existente dentro del servicio.</td>
    <td>Gestor</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>Suprimir una solución personalizada del servicio.</td>
    <td>Gestor</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>Visualizar las soluciones de socio que ha añadido a la instancia de servicio.</td>
    <td>Lector</br>Escritor</br>Gestor</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>Añadir una solución de socio a la instancia de servicio.</td>
    <td>Gestor</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>Actualizar una solución de socio en la instancia de servicio.</td>
    <td>Gestor</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>Suprimir una solución de socio de la instancia de servicio.</td>
    <td>Gestor</td>
  </tr>
</table>

## Correlaciones de API
{: #api-map}

¿Cómo se corresponden estos roles con la API? Consulte la tabla siguiente para ver cómo se correlacionan las acciones de servicio con la API.


| Método | Punto final                                                                  |  Acción de servicio                  |
|--------|---------------------------------------------------------------------------|----------------------------------|
| POST   | /v1/{account_id}/graph                                                    | security-advisor.findings.read   |
| POST   | /v1/{account_id}/providers/{provider_id}/notes                            | security-advisor.metadata.write  |
| GET    | /v1/{account_id}/providers/{provider_id}/notes                            | security-advisor.metadata.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.read   |
| PUT    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.update |
| DELETE | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.delete |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}/note | security-advisor.findings.read   |
| POST   | /v1/{account_id}/providers/{provider_id}/occurrences                      | security-advisor.findings.write  |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences                      | security-advisor.findings.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}/occurrences      | security-advisor.findings.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.read   |
| PUT    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.update |
| DELETE | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.delete |
