---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

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

# Network Insights
{: #setup-network}

Con {{site.data.keyword.security-advisor_long}}, puede supervisar el comportamiento mediante aprendizaje automático, patrones de comportamiento e inteligencia de amenazas para detectar los contenedores cuya seguridad se vea potencialmente comprometida que se ejecutan en los clústeres de {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

A partir del 20 de enero de 2019, Network Insights (beta) sustituye a la característica Network Analytics. Las tarjetas analíticas del panel de control del servicio se suprimen, pero los hallazgos permanecen en la base de datos de hallazgos.
{: important}

## Antes de empezar
{: #network-prereq}

Si actualmente utiliza la característica Network Analytics, debe [suprimir los componentes del servicio](/docs/services/security-advisor?topic=security-advisor-setup-network#uninstall-analytics) para poder instalar Network Insights. Para empezar a trabajar con Network Insights, asegúrese de cumplir los siguientes requisitos previos.

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
{: #network-setup-cos}

Con la GUI de {{site.data.keyword.security-advisor_short}}, puede crear una nueva instancia y un nuevo grupo de COS.

1. En el separador **Integraciones** del panel de control de servicio, cambie **Análisis inhabilitado** en el recuadro Network Insights por **Análisis habilitado**.

2. Pulse **Ir a configuración**.

3. En la sección de requisitos previos, pulse **Crear instancia y grupo de COS**. La instancia y el grupo de COS se crean automáticamente con el convenio de denominación y los permisos de IAM adecuados.

Si tiene una instancia y un grupo de COS existentes, asegúrese de que utilizan el convenio de denominación: `sa.<account_id>.telemetric.<cos_region>`. Para permitir que el servicio lea los datos almacenados en la instancia de COS, configure la [autorización de servicio a servicio](/docs/iam?topic=iam-serviceauth) mediante {{site.data.keyword.cloud_notm}} IAM. Establezca el valor de `source` en `{{site.data.keyword.security-advisor_short}}` y el de `target` en su instancia de COS. Asigne el rol de IAM de `Lector`.


## Instalación de componentes de {{site.data.keyword.security-advisor_short}}
{: #network-install-components}

Puede instalar un agente para que recopile registros del flujo de red del clúster de Kubernetes. Los registros se almacenan en la instancia de Cloud Object Storage. Luego puede habilitar Network Insights para que analice los registros y detecte y le alerte acerca de cualquier actividad de red sospechosa.
{: shortdesc}

Asegúrese de repetir la instalación para cada clúster que desee supervisar.
{: note}

1. Inicie una sesión en la CLI de {{site.data.keyword.cloud_notm}}. Siga las indicaciones de la CLI para completar el inicio de sesión.

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

2. Establezca el contexto para el clúster.

  1. Obtenga el mandato para establecer la variable de entorno y descargue los archivos de configuración de Kubernetes.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. Copie la salida comenzando por `export` y péguela en el terminal para establecer la variable de entorno `KUBECONFIG`.

3. Obtenga la versión de su clúster Kubernetes.

  ```
  kube_version=$(kubectl version --output json) echo $(echo $kube_version | yq r - serverVersion.major).$(echo $kube_version | yq r - serverVersion.minor)
  ```
  {: codeblock}

3. Si utiliza Kubernetes Service v1.12.x, instale Helm utilizando los mandatos siguientes. Si no, consulte la documentación de Kubernetes para ver los pasos de instalación que debe seguir para [configurar Helm en un clúster con acceso público](/docs/containers?topic=containers-helm#public_helm_install).

  1. Suprima los despliegues existentes.

    ```
    kubectl delete deployment tiller-deploy -n kube-system
    ```
    {: codeblock}

  2. Aplique las políticas de Tiller RBAC a su despliegue.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: codeblock}
  
  3. Inicialice Helm en su clúster e instale Tiller en su clúster.

    ```
    helm init --service-account tiller
    ```
    {: codeblock}

  4. Compruebe si su instalación es satisfactoria verificando que el estado del pod `tiller-deploy` es `running` (en ejecución).

    ```
    kubectl get pods -n kube-system -l app=helm
    ```
    {: codeblock}

4. Asegúrese de que Kubernetes Service es la versión 1.11 o posterior y después haga un clon del repositorio siguiente en su sistema local.

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-network-insights.git
  ```
  {: codeblock}

  Kubernetes Service versión 1.10 está en desuso y no se admite. Si ya tiene v1.10+ instalada, arregle las vulnerabilidades de la imagen existente reiniciando los pods del analizador ejecutando el siguiente mandato Helm: `helm upgrade --recreate-pods network-insights`.
  {: deprecated}

5. Vaya a la carpeta `security-advisor-network-insights`.

6. Vaya al directorio `v1.10+`.

7. Extraiga el archivo `.tar` con el mandato siguiente.

  ```
  tar -xvf security-advisor-network-insights.tar
  ```
  {: codeblock}

8. Vaya a la carpeta `security-advisor-network-insights`.

9. Instale Helm siguiendo la [documentación de integración del servicio Kubernetes](/docs/containers?topic=containers-helm).

10. Opcional: [Habilite TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md){: external}. Si utiliza su estación de trabajo para gestionar la instalación de componentes de análisis en varios clústeres y TLS está habilitado, asegúrese de que las configuraciones de TLS estén actualizadas y coincidan con el clúster actual donde tiene previsto instalar los componentes.

11. Ejecute el mandato siguiente para instalar el diagrama de Helm y sus dependencias. El mandato valida que el grupo utiliza el convenio de denominación correcto, crea secretos de Kubernetes, actualiza los valores con el GUID de clúster y despliega el diagrama de Helm de Network Insights. Si encuentra un error, intente ejecutar `helm init --upgrade`.

  ```
  ./network-insight-install.sh <cos_region> <cos_api_key>
  ```
  {: codeblock}

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
  </table>

Ha configurado correctamente el clúster para que funcione con {{site.data.keyword.security-advisor_short}} Network Insights.



## Supresión de los componentes
{: #network-delete}

Si ya no necesita utilizar Network Insights, puede suprimir del clúster los componentes del servicio.
{: shortdesc}

1. Inicie una sesión en la CLI de {{site.data.keyword.cloud_notm}}. Siga las indicaciones de la CLI para completar el inicio de sesión.

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

2. Establezca el contexto para el clúster.

  1. Obtenga el mandato para establecer la variable de entorno y descargue los archivos de configuración de Kubernetes.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. Copie la salida comenzando por `export` y péguela en el terminal para establecer la variable de entorno `KUBECONFIG`.

3. Suprima los componentes mediante Helm.

  ```
  helm del --purge network-insights [--tls]
  ```
  {: codeblock}

4. Suprima los secretos de Kubernetes.

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}

Asegúrese de suprimir el proceso para cada clúster del que desee eliminar agentes.
{: tip}

## Desinstalación de Network Analytics
{: #uninstall-analytics}

Si ha utilizado la versión beta de Network Analytics, debe desinstalar los componentes antiguos de {{site.data.keyword.security-advisor_short}} para poder instalar los nuevos. Asegúrese de repetir este proceso para cada clúster que contenga componentes de servicio.
{: shortdesc}

1. Inicie sesión en {{site.data.keyword.cloud_notm}}.

  ```
  ibmcloud login -a https://api.us-south.ibm.cloud.com --sso
  ```
  {: codeblock}

2. Obtenga una lista de todos los clústeres de la cuenta para obtener el nombre del clúster.

  ```
  ibmcloud ks clusters
  ```
  {: codeblock}

3. Establezca el contexto para el clúster.

  1. Obtenga el mandato para establecer la variable de entorno y descargue los archivos de configuración de Kubernetes.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. Copie la salida comenzando por `export` y péguela en el terminal para establecer la variable de entorno `KUBECONFIG`.

4. Vaya a la ubicación en que se ha extraído el archivo y ejecute el script del desinstalador.

  ```
  ./uninstall.sh
  ```
  {: codeblock}

5. Opcional: desinstale el componente de servidor Helm del clúster.

  ```
  helm reset
  ```
  {: codeblock}
