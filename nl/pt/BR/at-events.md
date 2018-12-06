---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-11"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Eventos do {{site.data.keyword.cloudaccesstrailshort}}
{: #at_events}

É possível visualizar, gerenciar e auditar as atividades iniciadas pelo usuário feitas na instância de serviço do {{site.data.keyword.security-advisor_long}} usando o serviço {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

Para obter mais informações sobre como o serviço funciona, consulte a [documentação do {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker/index.html).


## Onde visualizar os eventos
{: #monitor}

Os eventos estão disponíveis no **domínio de contas** do {{site.data.keyword.cloudaccesstrailshort}} que está acessível na região do {{site.data.keyword.Bluemix_notm}} em que os eventos são gerados.

1. Efetue login em sua conta do {{site.data.keyword.Bluemix_notm}}.
2. Por meio do catálogo, provisione uma instância do serviço {{site.data.keyword.cloudaccesstrailshort}} na mesma conta da instância do {{site.data.keyword.security-advisor_long}}.
3. Na guia **Gerenciar** do painel do {{site.data.keyword.cloudaccesstrailshort}}, clique em **Visualizar no Kibana**.
4. Configure o prazo durante o qual deseja visualizar os logs. O padrão é 15 min.
5. Na lista **Campos disponíveis**, clique em **tipo**. Clique no ícone de lupa para o **Activity Tracker** para limitar os logs somente aos rastreados pelo serviço.
6. É possível usar os outros campos disponíveis para limitar sua procura.

Para que usuários que não sejam o proprietário da conta visualizem os logs, o plano premium deve ser usado. Para permitir que outros usuários visualizem os eventos, consulte [Concedendo permissões para ver os eventos de conta](/docs/services/cloud-activity-tracker/how-to/grant_permissions.html#grant_permissions).
{: tip}

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
</table>
