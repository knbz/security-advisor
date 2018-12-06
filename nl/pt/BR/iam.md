---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Gerenciando o acesso de serviço
{: #service-access}

Como proprietário da conta, é possível gerenciar o acesso às instâncias do {{site.data.keyword.security-advisor_long}} usando o {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). Na sua conta, ao configurar políticas que criam diferentes níveis de acesso para diferentes usuários, é possível garantir que cada instância do {{site.data.keyword.security-advisor_short}} seja segura.
{: shortdesc}

Para obter mais informações sobre o IAM, veja [Acesso IAM](/docs/iam/users_roles.html).

## Políticas de acesso do
{{site.data.keyword.security-advisor_short}}
{: #access}

Todo usuário que acessar uma instância do serviço {{site.data.keyword.security-advisor_short}} na conta deverá ter uma política de acesso designada com uma função de usuário do IAM definida. A política determina quais ações um usuário pode executar dentro do contexto dessa instância de serviço específica.
{: shortdesc}

As ações são customizadas e definidas pelo serviço {{site.data.keyword.Bluemix_notm}} como operações que podem ser executadas no serviço. Em seguida, as ações são mapeadas para as funções de usuário do serviço IAM. Na tabela a
seguir, as ações e as permissões necessárias para o {{site.data.keyword.security-advisor_short}} são mapeadas.

<table><caption>Quais ações cada função pode executar?</caption>
  <col width="43%">
  <col width="42%">
  <col width="5%">
  <col width="5%">
  <col width="5%">
  <tr>
    <th>Ação</th>
    <th>Explicação</th>
    <th>Leitor</th>
    <th>Gravador</th>
    <th>Gerente</th>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>Visualizar as descobertas do relatório de segurança.</td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>Gerar as descobertas do relatório de segurança.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>Excluir as descobertas do relatório de segurança.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>Editar as descobertas do relatório de segurança.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>Visualizar os metadados.</td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>Excluir os metadados.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>Gerar os metadados.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>Atualizar os metadados.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>Ler as soluções customizadas que foram incluídas no serviço.</td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>Incluir uma solução customizada no serviço.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>Atualizar uma solução customizada existente dentro do serviço.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>Excluir uma solução customizada do serviço.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
</table>

## Mapeamentos de API
{: #api-map}

Como essas funções correspondem à API? Verifique a tabela a seguir para ver como as ações de serviço são mapeadas para a API.


| Método | Nó de Extremidade                                                                  |  Ação de serviço                  |
|--------|---------------------------------------------------------------------------|----------------------------------|
| POST   | /v1/{account_id}/graph                                                    | security-advisor.findings.read   |
| POST   | /v1/{account_id}/providers/{provider_id}/notes                            | security-advisor.metadata.write  |
| GET    | /v1/{account_id}/providers/{provider_id}/notes                            | security-advisor.metadata.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.read   |
| PUT    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.update |
| DELETE | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.delete |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}/note | security-advisor.findings.read   |
| POST   | /v1/{account_id}/providers/{provider_id}/occurrences                      | security-advisor.findings.write  |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences                      | security-advisor.findings.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}/occurrences      | security-advisor.findings.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.read   |
| PUT    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.update |
| DELETE | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.delete |
