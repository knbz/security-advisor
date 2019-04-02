---

copyright:
  years: 2019
lastupdated: "2019-02-18"

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
desenvolvimento do {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

* Se você tiver perguntas técnicas sobre o {{site.data.keyword.security-advisor_short}}, poste a sua pergunta no <a href="http://stackoverflow.com/" target="_blank">Stack Overflow <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. Certifique-se de incluir as tags `security-advisor` e `ibm-cloud`.
* Para perguntas sobre o serviço e instruções de introdução, use o fórum <a href="https://developer.ibm.com/" target="_blank">dW Answers <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. Certifique-se de incluir as tags `security-advisor` e `ibm-cloud`.

Para informações adicionais sobre como obter suporte, consulte [como obtenho o suporte de que preciso](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).


## Não é possível criar uma ocorrência de solução customizada
{: #ts-custom-occurrence}

{: tsSymptoms}
Você tenta criar uma ocorrência de solução customizada, mas as informações não são mostradas em um navegador e você não recebe nenhuma mensagem de erro.

{: tsCauses}
Você encontrou um problema conhecido. A criação da ocorrência falhou porque o nome escolhido já existe.

{: tsResolve}
Escolha outro nome para a ocorrência.


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
Antes de instalar uma das ofertas do Built-in Insights, deve-se instalar o Helm. É possível instalar o Helm usando os [docs de integração de serviço do Kubernetes](/docs/containers?topic=containers-integrations#helm).


## Defeito conhecido: algumas descobertas do Network Insights não são mostradas
{: #ts-network-analytics}

{: tsSymptoms}
Você não vê todos os tipos de descoberta quando procura o Network Insights por meio do painel ou da API.

{: tsCauses}
Alguns tipos de descobertas do Network Insights só funcionam no IBM Cloud Kubernetes Service versão 1.10 ou menos.

{: tsResolve}
Use o Kubernetes Service versão 1.10 ou menos.
