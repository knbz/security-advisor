---

copyright:
  years: 2018
lastupdated: "2018-12-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}

# Network Analytics (beta)
{: #network-analytics}


Com o {{site.data.keyword.security-advisor_long}}, é possível obter insight sobre a comunicação de rede potencialmente perigosa que está relacionada aos clusters do {{site.data.keyword.containerlong_notm}}. Para visualizar esse recurso, clique na seta no cartão Network Analytics na seção **Ferramentas do {{site.data.keyword.security-advisor_short}}** da [página **Recursos**](https://cloud.ibm.com/security-advisor#/capabilities).
{: shortdesc}

A visualização do Network Analytics está disponível apenas na região Sul dos EUA.
{: note}

O recurso de visualização Network Analytics consiste em três módulos:

1. **Agente de coleta de fluxo de rede**: instalado no cluster, o agente coleta as informações de rede e as envia para o back-end de análise de dados. Leia mais sobre [coleta de dados](#data-collection).

2. **Back-end de análise de dados**: o back-end de análise de dados identifica comunicação suspeita para e do cluster. Ele usa a inteligência de ameaça que é oferecida pelo [IBM X-Force![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/security/xforce) para interpretar as informações de rede. Quando uma comunicação suspeita é identificada, o back-end de análise de dados a monitora e envia as descobertas de segurança para o painel do {{site.data.keyword.security-advisor_short}}.

3. Painel do **{{site.data.keyword.security-advisor_short}}**: as descobertas e os KPIs do back-end de análise de dados são apresentados em dois cartões:

   - **Clientes suspeitos (tráfego recebido)**: esse cartão mostra os KPIs e as descobertas relacionados a clientes externos suspeitos que estão acessando o cluster, como quando um membro de um botnet aborda um dos IPs do cluster. Saiba mais sobre [clientes suspeitos](#suspicious-clients).
   - **IPs do servidor suspeitos (tráfego de saída)**: esse cartão mostra os KPIs e as descobertas que têm [IPs do servidor suspeitos](#suspicious-server-ips) que são executados como parte do cluster. Exemplo: quando o cluster aborda um servidor que é suspeito de distribuir malware.


## Coleta de dados
{: #data-collection}

O {{site.data.keyword.security-advisor_short}} pode ajudar a proteger o cluster incluindo o monitoramento de rede. Implementado como parte do cluster, o monitoramento de rede é usado para fornecer informações sobre as comunicações do cluster. Para ativar o Network Analytics, as informações de fluxo de rede são coletadas sobre a comunicação que ocorre entre o cluster e as entidades externas.
{: shortdesc}

As informações a seguir são coletadas:

* O endereço IP dos peers que estão se comunicando
* As portas que eles usam
* O conjunto de protocolos que está sendo usado
* A quantidade de dados que é transferida
* Um conjunto de parâmetros específicos do protocolo
* Vários registros de data e hora.

**Os dados reais que são trocados não são coletados**.

O monitoramento de rede funciona como parte do cluster com outros serviços, tal como o {{site.data.keyword.loganalysisshort_notm}}, para que você possa focar na lógica de negócios. É possível revisar os insights de monitoramento de rede no painel do {{site.data.keyword.security-advisor_short}}.


## Clientes suspeitos (tráfego recebido)
{: #suspicious-clients}

O painel do {{site.data.keyword.security-advisor_short}} inclui um cartão de cliente suspeito que resume várias informações sobre as comunicações nas quais o cluster atua como um servidor que é abordado por um cliente externo.
{: shortdesc}

A comunicação analisada pode produzir uma descoberta como:

- Um cliente externo com má reputação, como um cliente que é conhecido por distribuir malware ou está associado a serviços anônimos, aborda uma das URLs ou IPs associados do cluster. Por exemplo, um scanner aborda uma das portas abertas do cluster. Essa comunicação pode sugerir uma proteção inadequada das URLs e dos IPs associados do cluster, além de riscos pendentes ou reais.


## IPs do servidor suspeitos (tráfego de saída)
{: #suspicious-server-ips}

O painel do {{site.data.keyword.security-advisor_short}} inclui um cartão de IP do servidor suspeito que resume várias informações sobre as comunicações nas quais o cluster atua como um cliente que aborda um servidor externo.
{: shortdesc}

A comunicação analisada pode produzir uma descoberta como:

- Um cliente de dentro do cluster, como uma implementação de carga de trabalho, aborda um nó externo com má reputação, como um botnet. Essa comunicação pode sugerir que a carga de trabalho está comprometida e requer atenção da equipe de segurança. A descoberta varia com base na reputação do nó externo abordado, nos padrões de comunicação e no nível de certeza oferecido pela inteligência.

- A URL do cluster ou um de seus IPs tem má reputação. Revise uma má reputação para verificar se o cluster não está comprometido ou, de outra forma, envolvido em atividades inaceitáveis.
