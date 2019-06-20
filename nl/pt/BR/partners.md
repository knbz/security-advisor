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


# Parceiros
{: #setup-partner}

As integrações do Parceiro de Negócios IBM são uma maneira de trazer alertas críticos e descobertas de provedores de terceiro para o painel do {{site.data.keyword.security-advisor_long}}. Esses parceiros estão trabalhando com a IBM para ajudar a criar, simplificar e orientar a experiência de integração para você.
{: shortdesc}

## Antes de começar
{: #partner-before}

Antes de iniciar a integração de parceiros, certifique-se de que você tenha os pré-requisitos a seguir.

* Assegure-se de que tenha uma conta com o parceiro que deseja integrar.
* Assegure-se de que você tenha as permissões administrativas necessárias para gerar a URL de integração para o serviço de parceiro.
* Assegure-se de que você tenha acesso administrativo do IAM ao {{site.data.keyword.cloud_notm}} e acesso de gerenciador ao {{site.data.keyword.security-advisor_short}}.

## Assistente de integração
{: #wizard}

Como um administrador com permissões administrativas em ambas as contas do {{site.data.keyword.cloud_notm}} e do Parceiro, é possível usar o assistente de integração para funcionar e executar rapidamente. O assistente tem quatro etapas básicas:

* Estabelecer confiança e associar suas contas do {{site.data.keyword.cloud_notm}} e do parceiro
* Copiar as informações necessárias, tais como credenciais e URLs, entre as contas
* Instalar os metadados de descoberta do parceiro no {{site.data.keyword.security-advisor_short}}
* Validar o emparelhamento postando uma descoberta do parceiro no {{site.data.keyword.security-advisor_short}}


## Integrando o NeuVector
{: #setup-neuvector}

Com o [NeuVector](https://neuvector.com){: external}, é possível detectar ameaças de rede e violações para evitar ataques de seus aplicativos baseados em contêiner. Por meio de monitoramento, é possível evitar explosões e quebras detectando escalações de privilégios de administrador, varreduras de porta, shells reversos e atividade suspeita do sistema de arquivos em seus contêineres e hosts.
{: shortdesc}

Para integrar o NeuVector ao {{site.data.keyword.security-advisor_short}}, é possível usar as etapas a seguir:

1. Navegue para **Integrações > Integrações de parceiros** e selecione **NeuVector** nas opções fornecidas.
2. Conecte suas contas.
  1. Forneça um nome para sua conexão.
  2. Forneça a URL do painel do NeuVector.
  3. Forneça a URL de configuração do NeuVector.
    1. Efetue login no NeuVector e navegue para as configurações.
    2. Clique em **Gerar URL**.
    3. Copie a URL e cole-a no assistente de integração do {{site.data.keyword.security-advisor_short}}.
  4. Clique em **Avançar**.
3. Certifique-se de dar permissão para que o serviço gere um ID de serviço e uma chave de API e crie a conexão com o serviço clicando em **Avançar**.
4. Clique em **Pronto**.
5. Acesse seu painel de serviço para revisar uma descoberta de teste que foi enviada para o {{site.data.keyword.containershort_notm}} pelo NeuVector.



## Integrando o Twistlock
{: #setup-twistlock}

É possível evitar riscos parando a implementação das imagens vulneráveis em seu ambiente. Com o cumprimento de política automatizada e customizada, o [Twistlock](https://www.twistlock.com){: external} oferece controle completo em cada estágio do ciclo de vida do aplicativo.
{: shortdesc}

Quando você configura a conexão do parceiro, são criadas duas placas em seu painel que resumem as descobertas do Twistlock.

**Tempo de execução do Twistlock** apresenta dois Principais indicadores de risco (KRIs):

* Total de incidentes e auditorias: descobertas que estão relacionadas a incidentes ou auditorias em suas cargas de trabalho nativas de nuvem.
* Total de auditorias de firewall: descobertas que estão relacionadas a problemas com seu firewall.

**Vulnerabilidades do Twistlock** introduzem um KRI:

* Total de imagens com novas vulnerabilidades: descobertas que estão relacionadas a vulnerabilidades localizadas em suas imagens de contêiner.

É possível saber mais sobre a empresa nos docs do Twistlock.

### Configurando o Twistlock
{: #configure-twistlock}

1. No painel do {{site.data.keyword.security-advisor_short}}, navegue para **Integrações > Integrações de parceiros** e selecione **Twistlock** nas opções fornecidas.

2. Clique em  ** Sim, conecte minha conta ao  {{site.data.keyword.security-advisor_short}} **.

  Se você ainda não tiver uma conta, clique em **Não, ajude-me a criar uma conta > Criar uma conta**. É possível preencher suas informações e a equipe de vendas do Twistlock entrará em contato com você para que inicie.
  {: note}

3. Dê a sua conexão um nome. Certifique-se de que o nome seja exclusivo para sua conta e de que você use apenas caracteres alfanuméricos, espaço ou um hífen.

4. Opcional: se você ainda não tiver uma URL de configuração, clique em **Gerar URL de registro** para acessar a documentação do Twistlock e saber como criar a URL. Certifique-se de que você tenha o token do Twistlock que vem com sua licença para obter acesso aos docs.

5. No painel do Twistlock, navegue para a guia **Gerenciar > Alertas** e clique em **Incluir perfil**.

6. Selecione  ** {{site.data.keyword.security-advisor_short}} **  na lista suspensa  ** Provedor ** .

7. Clique em **Copiar** para usar a URL de configuração fornecida.

8. De volta no painel do {{site.data.keyword.security-advisor_short}}, cole a URL na caixa **Inserir a URL de configuração do Twistlock**.

9. Clique em  ** Conectar parceiro **.

### Verificando a Conexão
{: #twistlock-verify}

1. No painel do {{site.data.keyword.security-advisor_short}}, verifique se os cartões do Twistlock estão sendo exibidos conforme o esperado.

2. No painel do Twistlock, atualize a guia **Alertas**. A conexão do {{site.data.keyword.security-advisor_short}} é mostrada. Abra a conexão.

3. Verifique se os tipos de alerta que você deseja que sejam notificados estão marcados e, em seguida, clique em **Verificar** para assegurar que a conexão esteja concluída.
