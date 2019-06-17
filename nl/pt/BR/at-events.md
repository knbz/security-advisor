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



# Eventos do {{site.data.keyword.cloudaccesstrailshort}}
{: #at_events}

É possível visualizar, gerenciar e auditar as atividades iniciadas pelo usuário feitas na instância de serviço do {{site.data.keyword.security-advisor_long}} usando o serviço {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}






Para obter mais informações sobre como o serviço funciona, consulte os [docs do {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started#getting-started).



## Onde visualizar os eventos
{: #monitor}

Os eventos estão disponíveis no {{site.data.keyword.cloudaccesstrailshort}} **domínio de contas** que está disponível na região do {{site.data.keyword.cloud_notm}} na qual os eventos são gerados.

1. Efetue login em sua conta do {{site.data.keyword.cloud_notm}}.
2. Por meio do catálogo, provisione uma instância do serviço {{site.data.keyword.cloudaccesstrailshort}} na mesma conta da instância do {{site.data.keyword.security-advisor_short}}.
3. Na guia **Gerenciar** do painel do {{site.data.keyword.cloudaccesstrailshort}}, clique em **Visualizar no Kibana**.
4. Configure o prazo durante o qual deseja visualizar os logs. O padrão é 15 min.
5. Na lista **Campos disponíveis**, clique em **tipo**. Clique no ícone de lupa para o **Activity Tracker** para limitar os logs somente aos rastreados pelo serviço.
6. É possível usar os outros campos disponíveis para limitar sua procura.



## Lista de eventos
{: #events}

Confira a tabela a seguir para obter uma lista dos eventos enviados para o {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

<table>
  <tr>
    <th>Ação</th>
    <th>Descrição</th>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>Visualizar uma nota ou notas criadas anteriormente.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>Criar uma nota.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>Excluir uma nota.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>Atualizar uma nota.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>Visualizar uma ou mais ocorrências.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>Criar uma ocorrência.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>Excluir uma ocorrência.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>Atualizar uma ocorrência.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>Ler as soluções customizadas que foram incluídas no serviço.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>Incluir uma solução customizada no serviço.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>Atualizar uma solução customizada existente dentro do serviço.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>Excluir uma solução customizada do serviço.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>Visualizar as soluções de parceiros incluídas na instância de serviço.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>Incluir uma solução de parceiro na instância de serviço.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>Atualizar uma solução de parceiro na instância de serviço.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>Excluir uma solução de parceiro da instância de serviço.</td>
  </tr>
  <tr>
    <td><code> security-advisor.network-insights.enable </code></td>
    <td>Ative o recurso do Network Insights que é fornecido pelo {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code> security-advisor.network-insights.disable </code></td>
    <td>Desative o recurso do Network Insights que é fornecido pelo {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code> security-advisor.activity-insights.enable </code></td>
    <td>Ative o recurso Activity Insights que é fornecido pelo {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code> security-advisor.activity-insights.disable </code></td>
    <td>Desative o recurso Activity Insights que é fornecido pelo {{site.data.keyword.security-advisor_short}}.</td>
  </tr>
  <tr>
    <td><code> security-advisor.insights-cos.create </code></td>
    <td>Crie uma instância do Cloud Object Storage por meio do {{site.data.keyword.security-advisor_short}} para insights de rede e de atividade.</td>
  </tr>
</table>
