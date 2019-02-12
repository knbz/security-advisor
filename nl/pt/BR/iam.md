---

copyright:
  years: 2017, 2018
lastupdated: "2018-12-07"

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
  <col width="40%">
  <col width="40%">
  <col width="20%">
  <tr>
    <th>Ação</th>
    <th>Explicação</th>
    <th>Função</th>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>Visualizar as descobertas do relatório de segurança.</td>
    <td>Leitor</br>Gravador</br>Gerente</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>Gerar as descobertas do relatório de segurança.</td>
    <td>Gravador</br>Gerente</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>Excluir as descobertas do relatório de segurança.</td>
    <td>Gerente</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>Editar as descobertas do relatório de segurança.</td>
    <td>Gravador</br>Gerente</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>Visualizar os metadados.</td>
    <td>Leitor</br>Gravador</br>Gerente</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>Excluir os metadados.</td>
    <td>Gerente</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>Gerar os metadados.</td>
    <td>Gerente</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>Atualizar os metadados.</td>
    <td>Gerente</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>Ler as soluções customizadas que foram incluídas no serviço.</td>
    <td>Leitor</br>Gravador</br>Gerente</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>Incluir uma solução customizada no serviço.</td>
    <td>Gerente</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>Atualizar uma solução customizada existente dentro do serviço.</td>
    <td>Gerente</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>Excluir uma solução customizada do serviço.</td>
    <td>Gerente</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>Visualizar as soluções de parceiros incluídas na instância de serviço.</td>
    <td>Leitor</br>Gravador</br>Gerente</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>Incluir uma solução de parceiro na instância de serviço.</td>
    <td>Gerente</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>Atualizar uma solução de parceiro na instância de serviço.</td>
    <td>Gerente</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>Excluir uma solução de parceiro da instância de serviço.</td>
    <td>Gerente</td>
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
