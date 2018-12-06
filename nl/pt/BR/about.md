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

# Sobre {{site.data.keyword.security-advisor_short}}
{: #about}

Com o {{site.data.keyword.security-advisor_long}}, os administradores de segurança podem localizar, priorizar e gerenciar problemas de segurança em seus aplicativos em nuvem e cargas de trabalho.
{:shortdesc}

O {{site.data.keyword.security-advisor_short}} centraliza os serviços de insights, como o Vulnerability Advisor e o {{site.data.keyword.cloudcerts_short}} no {{site.data.keyword.Bluemix_notm}}. É possível gerenciar os eventos de segurança e aplicar a análise de dados para criar descobertas que unificam e melhoram os processos de gerenciamento de segurança no {{site.data.keyword.Bluemix_notm}}.

O {{site.data.keyword.security-advisor_short}} também oferece um recurso de análise de dados de rede de avaliação que é executado no cluster do {{site.data.keyword.containerlong_notm}} para reunir dados do fluxo de rede que podem detectar a comunicação com clientes e IPs do servidor suspeitos.

## Conceitos-chave
{: #concepts}

Conheça os diferentes conceitos que podem ser usados ao trabalhar com o {{site.data.keyword.security-advisor_short}}.
{: shortdesc}

<dl>
  <dt>Descoberta</dt>
    <dd>Uma descoberta é um problema de segurança prioritário que é criado quando os eventos brutos são processados. As descobertas são compostas das partes principais das informações necessárias para identificar questões relacionadas a quem, o quê, quando e onde do problema. Como um administrador de segurança, é possível usar as descobertas do {{site.data.keyword.security-advisor_short}} para priorizar e reagir às situações detectadas.</dd>
  <dt>Principal indicador de desempenho (KPI)</dt>
    <dd>Um principal indicador de desempenho é acionado quando o valor de uma descoberta está fora dos limites do intervalo de desempenho aceitável para controles de segurança específicos em serviços e cargas de trabalho.</dd>
  <dt>Nota</dt>
    <dd>É possível criar notas para categorizar as descobertas com as quais você se depara durante a análise. Uma nota pode ocorrer múltiplas vezes em diferentes provedores.</dd>
  <dt>Ocorrência</dt>
    <dd>Uma ocorrência descreve os detalhes específicos de uma nota do provedor. A ocorrência contém os detalhes de vulnerabilidade, as etapas de correção e outras informações gerais.</dd>
  <dt>CRN do serviço</dt>
    <dd>O CRN do serviço identifica o serviço do {{site.data.keyword.Bluemix_notm}} que está envolvido na descoberta.</br>
      <table>
        <tr>
          <th>Tipo de descoberta</th>
          <th>CRN</th>
        </tr>
        <tr>
          <td>{{site.data.keyword.cloudcerts_short}}</td>
          <td>O CRN da instância de serviço.</td>
        </tr>
        <tr>
          <td>Network Analytics</td>
          <td>O CRN do cluster do Kubernetes.</td>
        </tr>
        <tr>
          <td>Consultor de vulnerabilidade</td>
          <td>O CRN do contêiner.</td>
        </tr>
      </table></dd>
    <dt>CRN do recurso</dt>
      <dd>O CRN do recurso identifica o recurso específico envolvido na descoberta.</dd>
</dl>
