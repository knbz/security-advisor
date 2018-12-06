---

copyright:
  years: 2018
lastupdated: "2018-11-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}

# Analítica de red
{: #network-analytics}

Con {{site.data.keyword.security-advisor_long}}, puede obtener información detallada sobre la comunicación de red potencialmente peligrosa relacionada con los clústeres de {{site.data.keyword.containerlong_notm}}. Para ver una visión general de eta función de analítica de red, seleccione **Vista previa** en la {{site.data.keyword.security-advisor_short}} [página de iniciación](https://console.bluemix.net/security/advisor/#!/overview).
{: shortdesc}

La característica vista previa de analítica de red consta de tres módulos:

1. **Agente de recopilación de flujo de red**: instalado en el clúster, el agente recopila información de red y la envía al programa de fondo de análisis. Obtenga más información sobre la [recopilación de datos](#data-collection).

2. **Programa de fondo de análisis**: el programa de fondo de análisis identifica la comunicación sospechosa destinada al clúster y procedente de este. Utiliza la capacidad de inteligencia de detección de amenazas que ofrece [IBM X-Force![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/security/xforce) para interpretar la información de red. Cuando se identifica una comunicación sospechosa, el programa de fondo de análisis supervisa dicha comunicación y envía los hallazgos de seguridad al panel de control de {{site.data.keyword.security-advisor_short}}.

3. **Panel de control de {{site.data.keyword.security-advisor_short}}**: los hallazgos y KPI procedentes del programa de fondo de análisis se presentan en dos tarjetas:

   - **Clientes sospechosos (tráfico de entrada)**: esta tarjeta muestra los KPI y los hallazgos relacionados con clientes externos sospechosos que acceden al clúster, como por ejemplo cuando un miembro de una botnet se acerca a una de las IP del clúster. Obtenga más información sobre los [clientes sospechosos](#suspicious-clients).
   - **IP de servidor sospechosas (tráfico de salida)**: esta tarjeta muestra los KPI y los hallazgos que tienen [IP de servidor sospechosas](#suspicious-server-ips) que se ejecutan como parte del clúster. Ejemplo: cuando el clúster se acerca a un servidor sospechoso de distribuir software malicioso.


## Recopilación de datos
{: #data-collection}

{{site.data.keyword.security-advisor_short}} puede ayudar a proteger el clúster añadiendo supervisión de red. La supervisión de red, que se despliega como parte del clúster, se utiliza para proporcionar información sobre las comunicaciones del clúster. Para habilitar la analítica de red, se recopila información del flujo de la red sobre la comunicación que tiene lugar entre el clúster y entidades externas.
{: shortdesc}

Se recopila la información siguiente:

* La dirección IP de los iguales que se están comunicando
* Los puertos que utilizan
* El conjunto de protocolos que se están utilizando
* La cantidad de datos que se transfieren
* Un conjunto de parámetros específicos del protocolo
* Varias indicaciones de fecha y hora.

**Los datos reales que se intercambian no se recopilan**.

La supervisión de red funciona como parte del clúster con otros servicios como, por ejemplo {{site.data.keyword.loganalysisshort_notm}}, para que pueda centrarse en la lógica empresarial. Puede revisar información detallada sobre la supervisión de red en el panel de control de {{site.data.keyword.security-advisor_short}}.


## Clientes sospechosos (tráfico de entrada)
{: #suspicious-clients}

El panel de control de {{site.data.keyword.security-advisor_short}} incluye una tarjeta de clientes sospechosos que resume información variada sobre las comunicaciones en las que el clúster actúa como un servidor al que se acerca un cliente externo.
{: shortdesc}

La comunicación analizada puede generar un hallazgo como, por ejemplo:

- Un cliente externo con mala reputación, como por ejemplo un cliente del que se sabe que distribuye software malicioso o que está asociado a servicios anónimos, se acerca a uno de los URL o IP asociados al clúster. Por ejemplo, un escáner se acerca a uno de los puertos abiertos del clúster. Esta comunicación podría sugerir una protección inadecuada de los URL e IP asociados al clúster, y también riesgos pendientes o reales.


## IP de servidores sospechosos (tráfico de salida)
{: #suspicious-server-ips}

El panel de control de {{site.data.keyword.security-advisor_short}} incluye una tarjeta de IP de servidores sospechosos que resume información variada sobre las comunicaciones en las que el clúster actúa como un cliente que se acerca a un servidor externo.
{: shortdesc}

La comunicación analizada puede generar un hallazgo como, por ejemplo:

- Un cliente de dentro del clúster, como por ejemplo un despliegue de carga de trabajo, se acerca a un nodo externo con mala reputación, como una botnet. Esta comunicación podría sugerir que la carga de trabajo está en peligro y requiere atención por parte del equipo de seguridad. El hallazgo varía en función de la reputación del nodo externo abordado, los patrones de comunicación y el nivel de certeza que ofrece el sistema de inteligencia.

- El URL de clúster o una de sus IP tiene mala reputación. Revise una mala reputación para comprobar que el clúster no esté en peligro ni involucrado en una actividad inaceptable.
