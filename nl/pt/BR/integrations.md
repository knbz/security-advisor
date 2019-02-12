---

copyright:
  years: 2018
lastupdated: "2018-12-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Integrações
{: #about}

Com o {{site.data.keyword.security-advisor_long}}, é possível integrar descobertas de seu IBM Cloud Services, de soluções de parceiros ou criar suas próprias integrações customizadas.
{: shortdesc}


## Descobertas pré-integradas
{: #built-in}

Com o {{site.data.keyword.security-advisor_long}}, é possível obter insight sobre problemas potenciais por meio de recursos de serviços integrados.
{: shortdesc}


O {{site.data.keyword.security-advisor_short}} pronto para uso integra-se ao:

* Certificate Manager para notificações relacionadas à expiração e ao ciclo de vida do certificado.
* Vulnerability Advisor para obter detalhes sobre vulnerabilidades de imagem e problemas de configuração.

Para saber mais, verifique [Aproveitando os serviços pré-integrados](setup.html).

</br>

## Soluções de parceiros
{: #partner}

Soluções de parceiros são uma maneira de aprimorar a segurança de suas implementações do IBM Cloud, criando um link entre o {{site.data.keyword.security-advisor_short}} e outras ferramentas de segurança fora do IBM Cloud.
{: shortdesc}

O {{site.data.keyword.security-advisor_short}} está atualmente em parceria com as empresas a seguir:

### NeuVector
{: #neuvector}

O [NeuVector](https://neuvector.com/) fornece uma plataforma de segurança altamente integrada e automatizada para o Kubernetes e o Red Hat OpenShift que permite monitorar e proteger o tráfego de rede do contêiner. Especificamente, tráfego de rede leste-oeste.

Com o NeuVector, é possível detectar ameaças e violações de rede para evitar ataques de seus aplicativos baseados em contêiner. Por meio de monitoramento, é possível evitar explosões e quebras detectando escalações de privilégios de administrador, varreduras de porta, shells reversos e atividade suspeita do sistema de arquivos em seus contêineres e hosts.

Para saber como configurar a instância do NeuVector com o assistente de integração, consulte [Soluções de parceiros](partners.html).

Você é um parceiro e está interessado em integrar sua solução ao {{site.data.keyword.security-advisor_short}}? Fale com nossa equipe entrando em contato com Matt Ward em wardm@us.ibm.com.
{: tip}

</br>

## Integrações customizadas
{: #custom}

Talvez você já tenha uma ferramenta de segurança da qual depende. É possível integrar essa ferramenta ao painel do {{site.data.keyword.security-advisor_short}} para que todas as suas informações de segurança sejam centralizadas em um local.
{: shortdesc}

**Integrando suas próprias ferramentas com a GUI**

Usando a GUI do {{site.data.keyword.security-advisor_short}}, é possível marcar suas ferramentas de segurança internas e externas para que você tenha um ponto de acesso a tudo do painel do {{site.data.keyword.security-advisor_short}}. Por exemplo, você poderia marcar PagerDuty.

**Integrando suas próprias ferramentas com a API**

Com a API de Descobertas, é possível integrar descobertas de suas ferramentas de segurança customizadas no painel do {{site.data.keyword.security-advisor_short}}. Poderia ser qualquer ferramenta customizada ou do parceiro que gere um alerta de segurança ou uma descoberta que também suporte a interação baseada em API.

**Introdução**

Para aprender a prática recomendada sobre como aproveitar as APIs e criar marcadores por meio da GUI, consulte [Ferramentas de segurança customizadas](/docs/services/security-advisor/custom.html).

</br>


## Network Analytics (visualização)
{: #network-analytics}

Com a visualização do Network Analytics, é possível detectar ameaças monitorando a sua comunicação do cluster do {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Ao monitorar a comunicação do cluster para ver se há alguma comunicação suspeita para seus clusters e a partir deles, é possível aproveitar a inteligência de ameaça oferecida pelo IBM X-Force para sinalizar quaisquer problemas em potencial. Quando uma comunicação suspeita é identificada, é emitida uma descoberta que você pode revisar e avaliar.

Para obter informações mais detalhadas sobre o Network Analytics, consulte [Network Analytics](network-analytics.html).
{: tip}

</br>
</br>
