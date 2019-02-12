---

copyright:
  years: 2018
lastupdated: "2018-12-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:note: .note}

# Configuración de Network Analytics (beta)
{: #cluster_install}

Puede probar la característica de análisis de red del servicio instalando un {{site.data.keyword.security-advisor_short}} en un clúster de {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Network Analytics es una característica de vista previa que sólo está disponible en la región EE.UU. Sur.
{: note}

Cuando configura {{site.data.keyword.security-advisor_short}} en un clúster, se realizan las siguientes acciones:

* Se recopilan y se utilizan metadatos del clúster para completar la instalación de direcciones IP de nodos trabajadores, subredes, URL y dirección IP de Ingress, CRN de clúster, punto final de Log Analysis, espacio y señal.
* Se generan un ID de servicio de Identity and Access Management (IAM) y una clave de API en la cuenta de {{site.data.keyword.Bluemix_notm}} para que {{site.data.keyword.security-advisor_short}} pueda identificar el clúster.
* Los componentes de {{site.data.keyword.security-advisor_short}} se despliegan en el espacio de nombres de **supervisión** en el clúster.

Si tiene más de un clúster, asegúrese de ejecutar la instalación para cada clúster para generar los ID de servicio y las claves de API para cada clúster.


¿Desea obtener más información acerca del funcionamiento del análisis de red en {{site.data.keyword.security-advisor_short}}? [Consulte la documentación](network-analytics.html).
{: tip}

</br>

## Requisitos previos
{: #cluster_prereqs}

Antes de empezar, asegúrese de que tiene los siguientes requisitos previos en vigor.
{: shortdesc}

El propietario de la cuenta debe completar los pasos de instalación. Si no es usted, compruebe quién es el propietario de su cuenta de {{site.data.keyword.Bluemix_notm}} y pídale que le ayude con la instalación.
{: tip}

A continuación, asegúrese de cumplir los siguientes requisitos previos:

* Una estación de trabajo de desarrollador Mac, Linux o [Windows 10](https://win10faq.com/install-run-ubuntu-bash-windows-10/).
* [Node.js](https://nodejs.org/en/) 6 o superior
* [jQ](https://stedolan.github.io/jq/download/)
* Las CLI y los plugins necesarios en la versión mínima necesaria:
  * [CLI de {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started) v0.6.5 o superior
  * [CLI de Kubernetes (kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.7 o superior</br> El instalador se ha probado con la versión estable predeterminada de {{site.data.keyword.containerlong_notm}}` v1.10.11 `. El instalador no se ha probado con la versión `v1.11.5` ni con la versión más reciente `v1.12.3`.
  * [Plugin de {{site.data.keyword.containerlong_notm}}](https://console.bluemix.net/docs/containers/cs_cli_install.html#cs_cli_install)
  * [Helm de Kubernetes (gestor de paquetes)](https://docs.helm.sh/using_helm/#from-script)
* [Un clúster Kubernetes gratuito o estándar](https://console.bluemix.net/containers-kubernetes/catalog/cluster) en la región **EE.UU. sur** de {{site.data.keyword.Bluemix_notm}}

¿Necesita ayuda para crear y configurar un clúster? Puede ejecutar esta [guía de aprendizaje](/docs/containers/cs_tutorials.html#cs_cluster_tutorial).
{: tip}

</br>

## Configuración de Helm
{: #login}

1.  Inicie sesión en {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud login -a https://api.ng.bluemix.net --sso
  ```
  {: pre}

2.  Liste todos los clústeres de la cuenta para obtener el nombre del clúster con el que desea trabajar.

  ```
  ibmcloud ks clusters
  ```
  {: pre}

3.  Elija el clúster como destino de la CLI.

  1. Ejecute el mandato siguiente para configurar el clúster. Va a utilizar la salida en el paso siguiente.

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

  2. Utilice la salida del mandato anterior para establecer la vía de acceso al archivo de configuración de Kubernetes local como variable de entorno.

  Ejemplo:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: screen}

4.  Instale el tiller en el clúster para poder trabajar con diagramas Helm.

  ```
  helm init
  ```
  {: pre}

Asegúrese de mantener abierta esta ventana de línea de mandatos a medida que continúa.
{: tip}

</br>

## Instalación de componentes de {{site.data.keyword.security-advisor_short}}
{: #cluster_components}

Cuando el clúster esté configurado para trabajar con Helm, puede instalar los componentes de {{site.data.keyword.security-advisor_short}}.
{: shortdesc}


Para instalar los componentes, continúe en la misma ventana de CLI y efectúe los pasos siguientes:

1. Descargue y extraiga el [paquete de instalación](https://github.com/IBM-Bluemix-Docs/security-advisor/blob/master/installation.tar.gz?raw=true).
2. En la misma ventana de CLI de la sección anterior, vaya a la ubicación en la que se ha extraído el archivo e instale el paquete.

  ```
  ./install.sh
  ```
  {: pre}

3.  Verifique que los componentes se hayan instalado correctamente comprobando el [panel de control de {{site.data.keyword.security-advisor_short}}](https://console.bluemix.net/security-advisor/#/dashboard).

</br>

## Eliminación de los componentes de {{site.data.keyword.security-advisor_short}}
{: #cluster_uninstall}

Elimine los componentes de {{site.data.keyword.security-advisor_short}} del clúster y el ID de servicio y la clave de API de la cuenta de {{site.data.keyword.Bluemix_notm}}.

1. Inicie sesión en {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud login -a https://api.ng.bluemix.net --sso
  ```
  {: pre}

2. Obtenga una lista de todos los clústeres de la cuenta para obtener el nombre del clúster.

  ```
  ibmcloud ks clusters
  ```
  {: pre}

3. Elija el clúster como destino de la CLI.

  ```
  ibmcloud ks cluster-config <cluster-name-or-id>
  ```
  {: pre}

4. Establezca la vía de acceso al archivo de configuración de Kubernetes local como variable de entorno. Ejemplo:

  ```
  export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
  ```
  {: pre}

5. Vaya a la ubicación en que se ha extraído el archivo y ejecute el script del desinstalador.

  ```
  ./uninstall.sh
  ```
  {: pre}

6. Opcional: desinstale el componente de servidor Helm del clúster.

  ```
  helm reset
  ```
  {: pre}
