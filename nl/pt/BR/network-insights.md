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

Com o {{site.data.keyword.security-advisor_long}}, é possível obter insights com base em inteligência de ameaça, padrões comportamentais e aprendizado de máquina para contêineres potencialmente comprometidos que são executados nos seus clusters do {{site.data.keyword.containerlong_notm}}.
{: shortdesc}


## Como ele funciona
{: #network-how-it-works}

O recurso Network Insights é um complemento para o serviço {{site.data.keyword.security-advisor_short}}. Com o recurso ativado e configurado, a comunicação de rede de cluster, tanto de entrada quanto de saída, entre seu cluster e entidades externas é coletada e continuamente monitorada e analisada.

Veja o fluxo de informações na imagem a seguir.

![Network Insights flow diagram](images/network-insights-flow.png)

1. Como um administrador de conta, é possível instalar o Network Insights em cada cluster que você deseja monitorar.
2. Os logs do fluxo de rede são encaminhados para um depósito do Cloud Object Storage no qual eles são armazenados até que você decida excluí-los. Quando você usa a GUI do {{site.data.keyword.security-advisor_short}} para criar o depósito, as funções do IAM de serviço a serviço são designadas de modo que o serviço possa visualizar os logs.
3. Com o Network Insights ativado, os dados brutos em seu depósito COS são analisados por comportamento suspeito.
4. Quando um possível problema de segurança é sinalizado, a descoberta é encaminhada para o banco de dados Descobertas.
5. As descobertas são exibidas em seu painel de serviço em três cartões:
  * Tráfego de Entrada Suspeita
  * Tráfego de Saída Suspeita
  * Anomalias no tráfego


## Coleta de dados
{: #network-data}

O {{site.data.keyword.security-advisor_short}} fornece um coletor que é usado para reunir informações do fluxo de rede que ocorre entre um cluster e entidades externas.
{: shortdesc}

Os dados brutos que são coletados são armazenados em um depósito do Cloud Object Storage, no qual é possível determinar o período de tempo em que ele é armazenado. Você possui os dados coletados e os controla, o que significa que você é responsável por armazená-lo, protegê-los e excluí-los. O {{site.data.keyword.security-advisor_short}}  mantém as descobertas por 90 dias. Durante esse tempo, os resultados são apresentados nos cartões do Network Insights no painel de serviço. Portanto, embora você não veja mais a descoberta em seu painel após 90 dias, ainda é possível ter os dados brutos no armazenamento.

As informações a seguir são coletadas:

* O endereço IP dos peers que estão se comunicando
* As portas que eles usam
* O conjunto de protocolos que está sendo usado
* A quantidade de dados que é transferida
* Um conjunto de parâmetros específicos do protocolo
* Vários registros de data e hora.

**Os dados reais que são trocados não são coletados.**

Do ponto de vista de segurança, é uma boa ideia limpar seus dados coletados quando os requisitos legais ou da empresa permitem que eles sejam excluídos. Para obter mais informações, confira [Excluindo objetos](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion).
{: tip}


## Rede: Tráfego de Entrada Suspeita
{: #network-suspicious-inbound}

O {{site.data.keyword.security-advisor_short}} informa sobre as tentativas que são feitas por clientes externos para pesquisar e comprometer clusters no cartão **Tráfego de entrada suspeito** no painel de serviço.
{: shortdesc}


Os padrões comportamentais de clientes que são classificados pelo IBM X-Force como distribuindo malware que é usado como scanners, como parte de um botnet, para a criptografia de mineração ou para serviços de anonimização são todos monitorados continuamente. Se esse tipo de cliente se aproximar de seu cluster e exibição, abordar um cluster monitorado e exibir um comportamento alarmante, o Network Insights emitirá uma descoberta.


O cartão introduz dois Indicadores-chaves de risco (KRIs):

* Reconhecimento por clientes suspeitos: inclui descobertas que estão relacionadas a clientes que acessam o cluster.
* Cargas úteis anormalmente grandes enviadas por clientes suspeitos: inclui descobertas que estão relacionadas a volumes de dados que são enviados entre clientes e o cluster. Uma carga útil anormal é qualquer coisa que seja diferente do usual para seu cluster.


As descobertas podem incluir clientes suspeitos que:

* enviam quantidades anormalmente grandes de dados para o cluster;
* executam um número anormalmente grande de solicitações para um serviço de cluster;
* parecem destinar o cluster conforme exibido pelo número de tentativas que ele executa para pesquisar o cluster.



## Rede: Tráfego de Saída Suspeita
{: #network-suspicious-outbound}

O {{site.data.keyword.security-advisor_short}} informa sobre quaisquer contêineres potencialmente comprometidos que são executados em seus clusters do {{site.data.keyword.containershort_notm}} no cartão **Tráfego de saída suspeito** no painel de serviço.
{: shortdesc}

O serviço monitora continuamente os padrões comportamentais de contêineres que acessam clientes que são classificados pelo IBM X-Force como distribuindo malware que é usado como scanners, como parte de um botnet, para a criptografia de mineração de mineração ou para serviços de anonimização. Depois que um contêiner em um cluster monitorado se aproxima dos peers suspeitos e exibe o comportamento alarmante, o Network Insights emite uma descoberta.

O cartão introduz dois Indicadores-chaves de risco (KRIs):

* Abordagens de saída para servidores suspeitos: descobertas relacionadas a um cluster que acessa os servidores.
* Cargas úteis anormalmente grandes trocadas com servidores suspeitos: descobertas que estão relacionadas a volumes de dados que são enviados entre o cluster e servidores.


As descobertas podem incluir contêineres que:

* enviam quantidades anormalmente grandes de dados para um servidor suspeito;
* extraem uma grande quantidade de dados de um servidor suspeito;
* executam um grande número de solicitações para um servidor suspeito.


## Rede: Anomalias no tráfego (experimental)
{: #network-anomalies}

Nesse recurso experimental, o {{site.data.keyword.security-advisor_short}} monitora a sua rede para aprender padrões comportamentais. Após os padrões serem formados, qualquer comportamento que parecer anormal é sinalizado e resumido no painel de serviço no cartão **Anomalias no tráfego**.
{: shortdesc}

Um modelo de comportamento de contêiner normal é criado monitorando os padrões comportamentais entre um contêiner e seus peers. Quando o modelo é capturado, ele é monitorado para mudanças específicas. Se uma mudança alarmante for exibida, o Network Insights emitirá uma descoberta.

O cartão introduz dois Indicadores-chaves de risco (KRIs):

* Atividade de reconhecimento ou troca de dados mais alta que o normal: descobertas que estão relacionadas a interações anormais que são detectadas entre o cluster e quaisquer peers externos.
* Abordagem de saída para um novo servidor: descobertas que estão relacionadas a novos servidores detectados que o cluster se aproxima.

Uma descoberta pode incluir:  

* contêineres que abordam servidores que nunca foram abordados antes;
* contêineres que enviam ou consomem significativamente mais dados do que o normal para os peers específicos ou de peers específicos;
* o nível de pesquisa de um determinado contêiner aumentou significativamente.

 Após o modelo ser desenvolvido, os desvios do modelo aprendido são detectados e quando uma mudança alarmante é exibida, o Network Insights posta uma descoberta no painel do Security Advisor. As descobertas são resumidas no cartão 'Rede: anomalias no tráfego'. O cartão apresenta dois Principais indicadores de risco (KRIs). O KRI 'Atividade de reconhecimento ou troca de dados mais alta que o normal' conta as descobertas que estão relacionadas a interações anormais detectadas entre o cluster e os peers externos, enquanto o KRI 'Abordagem de saída para um novo servidor' conta as descobertas que estão relacionadas a novas abordagens de servidores detectados pelo cluster.  

## Próximas Etapas
{: #network-next}

Pronto para iniciar? Efetue check-out  [ Ativando o Network Insights ](/docs/services/security-advisor/setup-network.html)!
