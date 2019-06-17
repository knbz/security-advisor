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

# Activity Insights
{: #setup-activity}

Con {{site.data.keyword.security-advisor_long}}, puede supervisar continuamente los registros de {{site.data.keyword.cloud_notm}} Activity Tracker para identificar comportamientos no autorizados o sospechosos y cambios en los recursos. Puede utilizar los paquetes de reglas basados en prácticas recomendadas que proporciona el servicio o puede crear sus propias reglas personalizadas.
{: shortdesc}


## Antes de empezar
{: #activity-prereq}

Para empezar a trabajar con Activity Insights, asegúrese de cumplir los siguientes requisitos previos.

- Si trabaja en Windows 10, active el subsistema Windows para Linux e instale un [shell de Ubuntu](https://win10faq.com/install-run-ubuntu-bash-windows-10/){: external}.
- Instale la CLI yq:
  * Para [macOS y Windows 10](http://mikefarah.github.io/yq/){: external}.
  * Para CentOS, Red Hat y Ubuntu, ejecute los siguientes mandatos para instalar la versión 1.15.
    ```
    wget https://github.com/mikefarah/yq/releases/download/1.15.0/yq_linux_amd64       
    mv yq_linux_amd64 yq   
    chmod +x yq    
    mv yq /usr/local/bin/     
    yq -V
    ```
    {: codeblock}     
- Binario de cURL actualizado: para CentOS y Red Hat, puede actualizar con el mandato `yum update -y nss curl libcurl`.
- La [CLI de {{site.data.keyword.cloud_notm}} y los plugins necesarios](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)
- La [CLI de Kubernetes](https://kubernetes.io/docs/tasks/tools/install-kubectl/){: external} v1.10.11 o posterior
- El [Helm de Kubernetes (gestor de paquetes)](/docs/containers?topic=containers-helm) v2.9.0 o posterior.
- Un clúster de Kubernetes estándar versión v1.10.11 o posterior


## Creación de un grupo de COS
{: #activity-setup-cos}

Con la GUI de {{site.data.keyword.security-advisor_short}}, puede crear una nueva instancia y un nuevo grupo de COS.

1. Vaya al separador **Integraciones** del panel de control de servicio.

2. Cambie **Análisis inhabilitado** en el recuadro Activity Insights por **Análisis habilitado**.

3. Pulse **Ir a configuración**.

4. En la sección de requisitos previos, pulse **Crear instancia y grupo de COS**. La instancia y el grupo de COS se crean automáticamente con el convenio de denominación y los permisos de IAM adecuados. Se muestra la información del grupo.

Si tiene una instancia y un grupo de COS existentes, asegúrese de que utilizan el convenio de denominación `sa.<account_id>.telemetric.<cos_region>`. Para permitir que el servicio lea los datos almacenados en la instancia de COS, configure la [autorización de servicio a servicio](/docs/iam?topic=iam-serviceauth) mediante {{site.data.keyword.cloud_notm}} IAM. Establezca el valor de `source` en `{{site.data.keyword.security-advisor_short}}` y el de `target` en su instancia de COS. Asigne el rol de IAM de `Lector`.


## Instalación de componentes de {{site.data.keyword.security-advisor_short}}
{: #activity-install-components}

Puede instalar un agente para que recopile registros de flujo de auditoría de la cuenta de {{site.data.keyword.cloud_notm}}. Los registros se almacenan en la instancia de Cloud Object Storage, donde puede habilitar Activity Insights para analizar los registros en busca de comportamientos sospechosos.
{: shortdesc}

1. Clone el siguiente repositorio en su sistema local.

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. Vaya a la carpeta `security-advisor-activity-insights`.

3. Extraiga el archivo `.tar` con el mandato siguiente.

  ```
  tar -xvf security-advisor-activity-insights.tar
  ```
  {: codeblock}

4. Vaya a la carpeta `security-advisor-activity-insights`.
5. Inicie una sesión en la CLI de {{site.data.keyword.cloud_notm}}. Siga las indicaciones de la CLI para completar el inicio de sesión.

  ```
  ibmcloud login -a cloud.ibm.com -r <region>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>Región</th>
      <th>Punto final</th>
    </tr>
    <tr>
      <td>Reino Unido</td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>EE. UU. sur</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

6. Establezca el contexto para el clúster.

  1. Obtenga el mandato para establecer la variable de entorno y descargue los archivos de configuración de Kubernetes.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. Copie la salida comenzando por `export` y péguela en el terminal para establecer la variable de entorno `KUBECONFIG`.

7. Instale Helm siguiendo la [documentación de integración del servicio Kubernetes](/docs/containers?topic=containers-helm).

8. Opcional: [Habilite TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md){: external}. Si utiliza su estación de trabajo para gestionar la instalación de componentes de análisis en varios clústeres y TLS está habilitado, asegúrese de que las configuraciones de TLS estén actualizadas y coincidan con el clúster actual donde tiene previsto instalar los componentes.

9. Ejecute el mandato siguiente para instalar Insights. El mandato valida el convenio de denominación del grupo, crea secretos de Kubernetes, actualiza los valores con el GUID del clúster y despliega Activity Insights.

  ```
  ./activity-insight-install.sh <cos_region> <cos_api_key> <at_region> <account_api_key> <account_spaces>
  ```
  {: codeblock}

  Si encuentra un error, intente ejecutar `helm init --upgrade`.
  {: tip}

  <table>
    <tr>
      <th>Variable</th>
      <th>Descripción</th>
    </tr>
    <tr>
      <td><code>cos_region</code></td>
      <td>La región en la que se despliega la instancia de COS. Las opciones incluyen: <code>us-south</code> y <code>eu-gb</code>.</td>
    </tr>
    <tr>
      <td><code>cos_api_key</code></td>
      <td>La [clave de API](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) que ha creado para acceder a la instancia y al grupo de COS. La clave debe tener el rol de `escritor` de la plataforma.</td>
    </tr>
    <tr>
      <td><code>at_region</code></td>
      <td>La región en la que ha creado la instancia y el grupo de COS. Las opciones incluyen: <code>us-south</code> y <code>eu-gb</code>.</td>
    </tr>
    <tr>
      <td><code>account_api_key</code></td>
      <td>La clave de API de la plataforma correspondiente a su cuenta de {{site.data.keyword.cloud_notm}}.</td>
    </tr>
    <tr>
      <td><code>account_spaces</code></td>
      <td>Una lista separada por comas de los GUID correspondientes a su cuenta de {{site.data.keyword.cloud_notm}}.</td>
    </tr>
  </table>


## Adición de paquetes de reglas a COS
{: #activity-adding-rules}

Un paquete de reglas es un archivo JSON que contiene una lista de las reglas que desea supervisar. Puede descargar paquetes de reglas o [crear los suyos propios](/docs/services/security-advisor?topic=security-advisor-activity#activity-packages). El motor de {{site.data.keyword.security-advisor_short}} valida que cada regla sigue la sintaxis correcta.
{: shortdesc}

1. Clone el siguiente repositorio para obtener varios paquetes de reglas preestablecidos. Se crea una carpeta en el sistema local con el nombre `security-advisor-activity-insights`.

  ```
  https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. Cree una carpeta local con el nombre `IBM.rules/activities`.

3. Copie los archivos JSON de `security-advisor-activity-insights/security-advisor-ata-rule-packages` a `IBM.rules/activities`.

4. Vaya al panel de control de {{site.data.keyword.cloud_notm}} y seleccione la instancia de servicio de COS asociada con Activity Insights.

5. En el separador **Grupos** del panel de control de servicio, seleccione el grupo que está asociado con Activity Insights.

5. En la página de inicio de la instancia de COS, pulse **Cargar** y seleccione **Carpetas**.

6. Si se le solicita, pulse **Instalar cliente de Aspera Connect**. Si no ve esta indicación significa que ya tiene el cliente instalado. Si tiene que instalar el cliente, repita el paso 5 cuando haya finalizado la instalación.

7. Seleccione la carpeta *IBM.rules/activities*.

8. Pulse **Cargar**.

¿Desea utilizar sus propios paquetes? Utilice uno de los archivos JSON como guía y cree las reglas que se ajusten a las necesidades de su organización. Después de crear el archivo, añádelo a la carpeta *IBM.rules/activities* en la instancia de COS. Para obtener más información acerca de los tipos de reglas y su formato, consulte [Visión general de los paquetes de reglas](/docs/services/security-advisor?topic=security-advisor-activity).
{: tip}


## Supresión de los componentes
{: #activity-delete}

Si ya no necesita utilizar Activity Insights, puede suprimir del clúster los componentes del servicio.
{: shortdesc}

1. Suprima los componentes del servicio mediante Helm. Asegúrese de utilizar el distintivo `-tls` si tiene TLS habilitado.

  ```
  helm del --purge activity-insights [--tls]
  ```
  {: codeblock}

2. Suprima los secretos de Kubernetes.

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}
