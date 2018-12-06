---

copyright:
  years: 2018
lastupdated: "2018-11-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Configuración de la supervisión de clientes e IP de servidor sospechosos para un clúster Kubernetes
{: #cluster_install}

Para probar la característica Network Analytics, instale los componentes de {{site.data.keyword.security-advisor_short}} en un clúster que se haya desplegado en {{site.data.keyword.containerlong_notm}}. A continuación, puede ver información detallada sobre la red y alertas en el panel de control de {{site.data.keyword.security-advisor_short}}.
{:shortdesc}

Esta característica de {{site.data.keyword.security-advisor_short}} es una presentación técnica previa.
{: tip}

¿Desea obtener más información acerca de Network Analytics en {{site.data.keyword.security-advisor_short}}? [Consulte la documentación](network-analytics.html).


## Requisitos previos
{: #cluster_prereqs}

Antes de empezar:

* Estación de trabajo de desarrollador Mac, Linux o Windows 10
  * Windows 10: [habilite la característica Linux Subsystem](https://win10faq.com/install-run-ubuntu-bash-windows-10/)
* [Node.js](https://nodejs.org/en/) 6 o superior
* [jQ](https://stedolan.github.io/jq/download/)
* [Interfaz de línea de mandatos de {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started) v0.6.5 o superior
* [Plugin de CLI de {{site.data.keyword.containerlong_notm}}](https://console.bluemix.net/docs/containers/cs_cli_install.html#cs_cli_install)
* [CLI de Kubernetes (kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.7 o superior
* [Helm de Kubernetes (gestor de paquetes)](https://docs.helm.sh/using_helm/#from-script)
* [Un clúster Kubernetes gratuito o estándar](https://console.bluemix.net/containers-kubernetes/catalog/cluster) en la región **EE.UU. sur** de {{site.data.keyword.Bluemix_notm}}
* Identifique el propietario de la cuenta de {{site.data.keyword.Bluemix_notm}} para completar los pasos de la instalación

## Inicio de sesión en el clúster
{: #login}

1.  Inicie sesión en {{site.data.keyword.Bluemix_notm}}.

    ```
    ibmcloud login -a https://api.ng.bluemix.net --sso
    ```
    {: pre}

2.  Obtenga una lista de todos los clústeres de la cuenta para obtener el nombre del clúster.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

3.  Elija el clúster como destino de la CLI.

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

4.  Establezca la vía de acceso al archivo de configuración de Kubernetes local como variable de entorno. Ejemplo:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: pre}

5.  Configure Helm en el clúster.

    ```
    helm init
    ```
    {: pre}

Mantenga esta ventana de línea de mandatos abierta y continúe.
{: tip}

## Instalación de los componentes de Security Advisor
{: #cluster_components}

El propietario de la cuenta de {{site.data.keyword.Bluemix_notm}} debe ejecutar el instalador.
{: tip}

Cuando configura {{site.data.keyword.security-advisor_short}}, se realizan las siguientes acciones:
1. Se recopilan y se utilizan metadatos del clúster para completar la instalación de direcciones IP de nodos trabajadores, subredes, URL y dirección IP de Ingress, CRN de clúster, punto final de Log Analysis, espacio y señal.
2. Se genera un ID de servicio IAM y una clave de API en la cuenta de {{site.data.keyword.Bluemix_notm}} para que Security Advisor pueda identificar el clúster. Si tiene más de un clúster, ejecute la instalación para cada clúster para generar los ID de servicio y las claves de API para cada clúster.
3. Los componentes de {{site.data.keyword.security-advisor_short}} se despliegan en el espacio de nombres de **supervisión** del clúster.

<br/>
Para instalar los componentes de {{site.data.keyword.security-advisor_short}}:

1.  Descargue y extraiga el [paquete de instalación](https://github.com/IBM-Bluemix-Docs/security-advisor/blob/master/installation.tar.gz?raw=true).
2.  Desde la misma línea de mandatos de la sección anterior, vaya a la ubicación en que se ha extraído el archivo e instale el paquete.

    ```
    ./install.sh
    ```
    {: pre}

3.  Verifique que los componentes se han instalado correctamente comprobando el [panel de control de {{site.data.keyword.security-advisor_short}}](https://console.bluemix.net/security-advisor/#/dashboard).

## Eliminación de los componentes de {{site.data.keyword.security-advisor_short}}
{: #cluster_uninstall}

Elimine los componentes de {{site.data.keyword.security-advisor_short}} del clúster y el ID de servicio y la clave de API de la cuenta de {{site.data.keyword.Bluemix_notm}}.

1.  Inicie sesión en {{site.data.keyword.Bluemix_notm}}.

    ```
    ibmcloud login -a https://api.ng.bluemix.net --sso
    ```
    {: pre}

2.  Obtenga una lista de todos los clústeres de la cuenta para obtener el nombre del clúster.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

3.  Elija el clúster como destino de la CLI.

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

4.  Establezca la vía de acceso al archivo de configuración de Kubernetes local como variable de entorno. Ejemplo:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: pre}

5.  Vaya a la ubicación en que se ha extraído el archivo y ejecute el script del desinstalador.

    ```
    ./uninstall.sh
    ```
    {: pre}

6.  Opcional: desinstale el componente de servidor Helm del clúster.

    ```
    helm reset
    ```
    {: pre}
