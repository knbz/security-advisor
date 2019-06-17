---

copyright:
  years: 2019
lastupdated: "2019-06-06"

---

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# Resolução de problemas
{: #troubleshooting}

Se você tiver problemas enquanto está trabalhando com o {{site.data.keyword.security-advisor_long}}, considere estas técnicas para resolução de problemas e obtenção de ajuda.
{: shortdesc}


## Obtendo ajuda e suporte
{: #getting-help-and-support}



É possível localizar ajuda procurando informações ou fazendo perguntas por meio de um fórum. Também é possível abrir um chamado de suporte. Quando estiver usando os fóruns para fazer uma pergunta, identifique sua pergunta para que ela seja vista pela equipe de
desenvolvimento do {{site.data.keyword.cloud_notm}}.
{: shortdesc}

  * Se você tiver perguntas técnicas sobre o {{site.data.keyword.security-advisor_short}}, poste a sua pergunta no [Stack Overflow](https://stackoverflow.com/){: external}. Certifique-se de incluir as tags `security-advisor` e `ibm-cloud`.

  * Para perguntas sobre o serviço e as instruções de introdução, use o fórum [dW Answers](https://developer.ibm.com/){: external}. Certifique-se de incluir as tags `security-advisor` e `ibm-cloud`.


Para informações adicionais sobre como obter suporte, consulte [como obtenho o suporte de que preciso](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).


## Erro: namespaces "security-advisor-insights" é proibido
{: #ts-error-helm-install}

{: tsSymptoms}
Quando você tenta instalar o Network ou o Activity Insights, você encontra o erro:

```
namespaces “security-advisor-insights” is forbidden: User “system:serviceaccount:kube-system:default” cannot get resource “namespaces” in API group “” in the namespace “security-advisor-insights”
```
{: screen}

{: tsCauses}
A conta de serviço padrão do `kube-system` não tem acesso de administrador em seu cluster.

{: tsResolve}
Antes de instalar uma das ofertas do Built-in Insights, deve-se instalar o Helm. É possível instalar o Helm usando os [docs de integração de serviço do Kubernetes](/docs/containers?topic=containers-helm).


## Problema conhecido: não é possível criar uma ocorrência de solução customizada
{: #ts-custom-occurrence}

{: tsSymptoms}
Você tenta criar uma ocorrência de solução customizada, mas as informações não são mostradas em um navegador e você não recebe nenhuma mensagem de erro.

{: tsCauses}
A criação da ocorrência falhou porque o nome que você escolheu está em uso.

{: tsResolve}
Escolha outro nome para a ocorrência.

## Problema conhecido: a imagem de conexão direta não é atualizada
{: #ts-known-cant-edit}

{: tsSymptoms}
Quando você faz upload ou edita uma imagem para uma conexão direta, ela não é exibida imediatamente.

{: tsCauses}
É um problema conhecido que as imagens não estão sendo exibidas corretamente.

{: tsResolve}
Para ver a imagem atualizada, atualize o seu navegador. A nova imagem é exibida na tabela de conexão direta.

