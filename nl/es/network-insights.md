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


# Network Insights (beta)
{: #network}

Con {{site.data.keyword.security-advisor_long}}, puede obtener información detallada sobre la base de inteligencia de amenazas, patrones de comportamiento y aprendizaje automático para detectar los contenedores cuya seguridad se vea potencialmente comprometida que se ejecutan en los clústeres de {{site.data.keyword.containerlong_notm}}.
{: shortdesc}


## Funcionamiento
{: #network-how-it-works}

La característica Network Insights es un complemento del servicio {{site.data.keyword.security-advisor_short}}. Con la característica habilitada y configurada, se supervisa y se analiza continuamente la comunicación de red de clúster, tanto de entrada como de salida, entre el clúster y entidades externas.

Consulte la imagen siguiente para ver el flujo de información.

![Diagrama de flujo de Network Insights](images/network-insights-flow.png)

1. Como administrador de la cuenta, puede instalar Network Insights en cada clúster que desee supervisar.
2. Los registros de flujo de red se reenvían a un grupo de Cloud Object Storage (COS), donde se almacenan hasta que decide suprimirlos. Si utiliza la GUI de {{site.data.keyword.security-advisor_short}} para crear el grupo, se asignan roles de IAM de servicio a servicio para que el servicio pueda ver los registros.
3. Con la característica Network Insights habilitada, los datos sin procesar del grupo de COS se analizan en busca de comportamientos sospechosos.
4. Cuando se detecta un posible problema de seguridad, el hallazgo se reenvía a la base de datos de hallazgos.
5. Los hallazgos se muestran en el panel de control de servicio en tres tarjetas:
  * Tráfico de entrada sospechoso
  * Tráfico de salida sospechoso
  * Anomalías en el tráfico


## Recopilación de datos
{: #network-data}

{{site.data.keyword.security-advisor_short}} proporciona un recopilador que se utiliza para recopilar información de flujo de red que tiene lugar entre un clúster y entidades externas.
{: shortdesc}

Los datos sin procesar que se recopilan se almacenan en un grupo de Cloud Object Storage, donde puede determinar el intervalo de tiempo durante el cual se almacenan. El usuario es el propietario de los datos recopilados y los controla, lo que significa que es el responsable de almacenarlos, protegerlos y suprimirlos. {{site.data.keyword.security-advisor_short}} mantiene los hallazgos durante 90 días. Durante ese tiempo, los resultados se presentan en las tarjetas de Network Insights en el panel de control de servicio. Por lo tanto, aunque ya no verá el hallazgo en el panel de control transcurridos 90 días, es posible que todavía tenga los datos sin procesar en el almacenamiento.

Se recopila la información siguiente:

* La dirección IP de los iguales que se están comunicando
* Los puertos que utilizan
* El conjunto de protocolos que se están utilizando
* La cantidad de datos que se transfieren
* Un conjunto de parámetros específicos del protocolo
* Varias indicaciones de fecha y hora.

**Los datos reales que se intercambian no se recopilan.**

Desde un punto de vista de seguridad, se recomienda eliminar los datos recopilados cuando los requisitos legales o de la empresa permitan que se supriman. Para obtener más información, consulte [Supresión de objetos](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion).
{: tip}


## Red: Tráfico de entrada sospechoso
{: #network-suspicious-inbound}

{{site.data.keyword.security-advisor_short}} informa de los intentos realizados por clientes externos de inspeccionar y poner en peligro los clústeres en la tarjeta **Tráfico de entrada sospechoso** del panel de control de servicio.
{: shortdesc}


Se supervisan continuamente todos los patrones de comportamiento de los clientes clasificados por IBM X-Force como distribuidores de malware que se utiliza como exploradores, como parte de un botnet, como autores de minería de criptomoneda o como servicios de anonimización. Si este tipo de cliente se acerca a su clúster y muestra un comportamiento alarmante, la característica Network Insights emite un hallazgo.


La tarjeta incorpora dos indicadores de riesgo clave (KRI):

* Reconocimiento por parte de clientes sospechosos: incluye hallazgos relacionados con clientes que acceden al clúster.
* Cargas útiles anormalmente grandes enviadas por clientes sospechosos: incluye hallazgos relacionados con volúmenes de datos que se envían entre clientes y el clúster. Una carga útil anormal es cualquier cosa que queda fuera del comportamiento normal del clúster.


Los hallazgos pueden incluir clientes sospechosos que:

* Envían cantidades anormalmente grandes de datos al clúster.
* Realizar un número anormalmente elevado de solicitudes a un servicio del clúster.
* Parecen utilizar el clúster como destino, según muestra el número de intentos que realizan de inspeccionar el clúster.



## Red: Tráfico de salida sospechoso
{: #network-suspicious-outbound}

{{site.data.keyword.security-advisor_short}} le informa sobre cualquier contenedor cuya seguridad se vea potencialmente comprometida que se ejecute en los clústeres de {{site.data.keyword.containershort_notm}} en la tarjeta **Tráfico de salida sospechoso** del panel de control de servicio.
{: shortdesc}

El servicio supervisa continuamente los patrones de comportamiento de los contenedores a los que acceden los clientes clasificados por IBM X-Force como distribuidores de malware que se utiliza como exploradores, como parte de un botnet, como autores de minería de criptomoneda o como servicios de anonimización. Si un contenedor de un clúster supervisado se aproxime a los iguales sospechosos y muestra un comportamiento alarmante, la característica Network Insights emite un hallazgo.

La tarjeta incorpora dos indicadores de riesgo clave (KRI):

* Enfoques de salida a servidores sospechosos: hallazgos relacionados con un clúster que accede a los servidores.
* Cargas útiles anormalmente grandes intercambiadas con servidores sospechosos: hallazgos relacionados con volúmenes de datos que se envían entre el clúster y los servidores.


Los hallazgos pueden incluir contenedores que:

* envían cantidades anormalmente grandes de datos a un servidor sospechoso
* extraen una gran cantidad de datos de un servidor sospechoso
* realizan un gran número de solicitudes a un servidor sospechoso


## Red: Anomalías en el tráfico (experimental)
{: #network-anomalies}

En esta característica experimental, {{site.data.keyword.security-advisor_short}} supervisa la red para detectar patrones de comportamientos. Una vez definidos los patrones, cualquier comportamiento que parezca estar fuera de lo normal se marca y se resume en el panel de control de servicio en la tarjeta **Anomalías en el tráfico**.
{: shortdesc}

Se crea un modelo de comportamiento normal de contenedor mediante la supervisión de los patrones de comportamiento entre un contenedor y sus iguales. Cuando se captura el modelo, se supervisa para detectar cambios específicos. Si se muestra un cambio alarmante, la característica Network Insights emite un hallazgo.

La tarjeta incorpora dos indicadores de riesgo clave (KRI):

* Actividad de reconocimiento o de intercambio de datos más alta de lo normal: hallazgos relacionados con interacciones anómalas detectadas entre el clúster y cualquier igual externo.
* Enfoque de salida a un nuevo servidor: hallazgos relacionados con servidores recién detectados a los que se acerca el clúster.

Un hallazgo puede incluir:  

* contenedores que se aproximan a servidores a los que nunca se habían acercado anteriormente
* contenedores que envían a iguales específicos muchos más datos de lo normal o que los consumen de los mismos
* el nivel de inspección de un servidor en concreto aumenta significativamente

 Una vez que se ha desarrollado el modelo, se detectan las desviaciones con respecto al modelo aprendido, y, cuando se muestra un cambio alarmante, Network Insights publica un hallazgo en el panel de control de Security Advisor. Los hallazgos se resumen en la tarjeta "Red: Anomalías en el tráfico". La tarjeta incorpora dos indicadores de riesgo clave (KRI). El KRI 'Actividad de reconocimiento o de intercambio de datos más alta de lo normal' cuenta los hallazgos relacionados con interacciones anómalas detectadas entre el clúster y cualquier igual externo, mientras que el KRI 'Enfoque de salida a un nuevo servidor' cuenta los hallazgos relacionados con acercamientos a servidores recién detectados por parte del clúster.  

## Pasos siguientes
{: #network-next}

¿Listo para empezar? Consulte [Habilitación de Network Insights](/docs/services/security-advisor/setup-network.html).
