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

# Acerca de {{site.data.keyword.security-advisor_short}}
{: #about}

Con la seguridad que proporciona {{site.data.keyword.security-advisor_long}}, los administradores pueden encontrar, priorizar y gestionar los problemas de seguridad de las aplicaciones y cargas de trabajo de la nube.
{:shortdesc}

{{site.data.keyword.security-advisor_short}} centraliza los servicios de obtención de información detallada, como Vulnerability Advisor, y {{site.data.keyword.cloudcerts_short}} en {{site.data.keyword.Bluemix_notm}}. Puede gestionar los sucesos de seguridad y aplicar análisis para crear hallazgos que unifiquen y mejoren los procesos de gestión de seguridad en {{site.data.keyword.Bluemix_notm}}.

{{site.data.keyword.security-advisor_short}} también ofrece capacidad de análisis de red de prueba que se ejecuta en el clúster de {{site.data.keyword.containerlong_notm}} para recopilar datos de flujo de red que pueden detectar la comunicación con clientes e IP de servidor sospechosos.

## Conceptos clave
{: #concepts}

Obtenga más información sobre los distintos conceptos que puede utilizar cuando trabaje con {{site.data.keyword.security-advisor_short}}.
{: shortdesc}

<dl>
  <dt>Hallazgo</dt>
    <dd>Un hallazgo es un problema de seguridad de prioridades que se crea cuando se procesan sucesos en bruto. Los hallazgos están hechos de las partes clave de la información que se necesitan para identificar quién, qué, cuándo y dónde en relación con el problema. Como administrador de seguridad, puede utilizar los hallazgos de {{site.data.keyword.security-advisor_short}} para priorizar y reaccionar ante situaciones detectadas.</dd>
  <dt>Indicador clave de rendimiento (KPI)</dt>
    <dd>Se desencadena un indicador clave de rendimiento cuando el valor de un hallazgo está fuera de los límites del rango de rendimiento aceptable para controles de seguridad específicos en servicios y cargas de trabajo.</dd>
  <dt>Nota</dt>
    <dd>Puede crear notas para categorizar los hallazgos que llegan mientras está analizando. Una nota se puede producir varias veces entre distintos proveedores.</dd>
  <dt>Aparición</dt>
    <dd>Una aparición describe los detalles específicos del proveedor de una nota. La aparición contiene los detalles de la vulnerabilidad, los pasos para solucionarla y otra información general.</dd>
  <dt>CRN de servicio</dt>
    <dd>El CRN de servicio identifica el servicio de {{site.data.keyword.Bluemix_notm}} involucrado en el hallazgo.</br>
      <table>
        <tr>
          <th>Tipo de hallazgo</th>
          <th>CRN</th>
        </tr>
        <tr>
          <td>{{site.data.keyword.cloudcerts_short}}</td>
          <td>El CRN de la instancia de servicio.</td>
        </tr>
        <tr>
          <td>Analítica de red</td>
          <td>El CRN del clúster de Kubernetes.</td>
        </tr>
        <tr>
          <td>Vulnerability Advisor</td>
          <td>El CRN del contenedor.</td>
        </tr>
      </table></dd>
    <dt>CRN de recurso</dt>
      <dd>El CRN del recurso identifica el recurso específico que está involucrado en el resultado.</dd>
</dl>
