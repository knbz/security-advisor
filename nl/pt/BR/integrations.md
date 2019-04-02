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


# Integrações
{: #integrations}

Com o {{site.data.keyword.security-advisor_long}}, é possível integrar outros insights integrados, soluções de parceiros ou criar suas próprias integrações customizadas.
{: shortdesc}


## Descobertas pré-integradas
{: #integrate-built-in}

Com o {{site.data.keyword.security-advisor_long}}, é possível obter insight sobre problemas potenciais por meio de recursos de serviços integrados.
{: shortdesc}


Fora da caixa, o  {{site.data.keyword.security-advisor_short}}  se integra ao:

* Certificate Manager para notificações relacionadas à expiração e ao ciclo de vida do certificado.
* Vulnerability Advisor para obter detalhes sobre vulnerabilidades de imagem e problemas de configuração.

Para saber mais, verifique [Aproveitando os serviços pré-integrados](/docs/services/security-advisor?topic=security-advisor-setup-services).

</br>

## Integrações de parceiros
{: #integrate-partner}

As integrações de parceiros são uma maneira de aprimorar a segurança para suas implementações do {{site.data.keyword.cloud_notm}} criando uma conexão entre o {{site.data.keyword.security-advisor_short}} e outras ferramentas de segurança que estão trabalhando com a IBM para assegurar uma experiência do usuário contínua.
{: shortdesc}

Os parceiros atuais do {{site.data.keyword.security-advisor_short}} incluem Neuvector e Twistlock.

Você é um parceiro e está interessado em integrar sua solução ao {{site.data.keyword.security-advisor_short}}? Fale com nossa equipe entrando em contato com Matt Ward em wardm@us.ibm.com.
{: tip}

### NeuVector
{: #integrate-neuvector}

O [NeuVector](https://neuvector.com/) fornece uma plataforma de segurança altamente integrada e automatizada para o Kubernetes e o Red Hat OpenShift que permite monitorar e proteger o tráfego de rede do contêiner. Especificamente, tráfego de rede leste-oeste.

Com o NeuVector, é possível detectar ameaças de rede e violações para impedir ataques de seus aplicativos baseados em contêiner. Por meio de monitoramento, é possível evitar explosões e quebras detectando escalações de privilégios de administrador, varreduras de porta, shells reversos e atividade suspeita do sistema de arquivos em seus contêineres e hosts.

Para obter ajuda com a configuração de sua instância do NeuVector, consulte [Soluções do parceiro](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-neuvector).


### Twistlock
{: #integrate-twistlock}

O Twistlock é exclusivamente capaz de impedir o risco em todo o SDLC, impedindo a implementação de imagens vulneráveis em seu ambiente. O cumprimento de política automatizada e customizada oferece controle completo em cada estágio do ciclo de vida do aplicativo.
O Twistlock Intelligence Stream origina e agrega informações de vulnerabilidade diretamente de mais de 30 projetos de envio de dados, origens comerciais e pesquisa de proprietário dos Twistlock Labs.

Com um foco em ter os dados mais precisos disponíveis para abranger todas as camadas de sua pilha, para que você tenha visibilidade precisa e a menor taxa de falsos positivos.O Twistlock combina esses dados com conhecimento de suas implementações reais, tais como quais contêineres são expostos à Internet, quais são executados com alto privilégio e quais possuem outras mitigações de segurança no local, para que você possa sempre ver quais vulnerabilidades são mais críticas em seu ambiente específico.

Para obter ajuda com a configuração de sua instância do Twistlock, consulte [Soluções de parceiros](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-twistlock).
</br>


## Integrações customizadas
{: #integrate-custom}

Talvez você já tenha uma ferramenta de segurança da qual depende. É possível integrar essa ferramenta ao painel do {{site.data.keyword.security-advisor_short}} para que todas as suas informações de segurança sejam centralizadas em um local.
{: shortdesc}

### Criando uma conexão direta
{: #create-direct-connect}

Usando a GUI do {{site.data.keyword.security-advisor_short}}, é possível marcar suas ferramentas de segurança internas e externas para que você tenha um ponto de acesso a tudo do painel do {{site.data.keyword.security-advisor_short}}. Por exemplo, você poderia marcar PagerDuty.

### Integrando suas próprias ferramentas com a API
{: #integrate-tools-api}

Com a API de Descobertas, é possível integrar descobertas de suas ferramentas de segurança customizadas no painel do {{site.data.keyword.security-advisor_short}}. Poderia ser qualquer ferramenta customizada ou de parceiro que gere um alerta de segurança ou descoberta que também suporte a interação baseada em API.

## Insights Integrados
{: #integrate-insights}

Com insights integrados, é possível detectar problemas potenciais ao monitorar continuamente seus logs do cluster e da conta. Ao monitorar o tráfego de rede e a atividade do usuário, é possível ajudar a assegurar que seus recursos do {{site.data.keyword.cloud_notm}} permaneçam protegidos.
{: shortdesc}

** Network Insights (beta) **

Com o Network Insights (beta), é possível monitorar e analisar a comunicação de rede de cluster, de entrada e de saída, entre o cluster do Kubernetes e entidades externas. Ao usar a inteligência de ameaça integrada e a detecção de anomalias, o serviço pode identificar ataques de reconhecimento e ativos potencialmente comprometidos. Para saber mais, verifique o [Network Insights](/docs/services/security-advisor?topic=security-advisor-network).

** Activity Insights (preview) **

Com o Activity Insights (preview), é possível monitorar continuamente seus logs do {{site.data.keyword.cloud_notm}} Activity Tracker para identificar atividades não autorizadas ou suspeitas que são feitas por usuários ou apps usando pacotes de regras. É possível usar os pacotes de regras fornecidos pelo serviço que são baseados nas melhores práticas de segurança ou é possível customizar as regras para atender às suas necessidades. Para saber mais, confira [Activity Insights](/docs/services/security-advisor?topic=security-advisor-activity).
