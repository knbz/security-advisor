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

# Activity Insights (preview)
{: #activity}

Com o {{site.data.keyword.security-advisor_long}}, é possível detectar atividade suspeita do usuário em sua conta do {{site.data.keyword.Bluemix_notm}} usando o {{site.data.keyword.cloud_notm}} Activity Tracker.
{: shortdesc}


## Como ele funciona
{: #activity-how}

O recurso Activity Insights é um complemento para o serviço {{site.data.keyword.security-advisor_short}}. Com o recurso ativado e configurado, o comportamento do usuário é registrado e analisado para identificar a atividade suspeita com base nas regras. É possível usar regras padrão ou é possível criar regras customizadas para ajustar à sua organização.

Veja o fluxo de informações na imagem a seguir.

![Activity Insights flow diagram](images/activity-insights-flow.png)

1. Como um administrador de conta, é possível instalar o Activity Insights em seu cluster.
2. Com o complemento instalado em um cluster, ele pode monitorar logs do Activity Tracker para sua conta inteira.
3. Os logs de atividades são encaminhados para um depósito do Cloud Object Storage no qual eles são armazenados até que você decida excluí-los. Quando você usa a GUI do {{site.data.keyword.security-advisor_short}} para criar o depósito, as funções do IAM de serviço a serviço são designadas de modo que o serviço possa visualizar os logs.
4. Com o Activity Insights ativado, os dados brutos em seu depósito COS são analisados com base em regras que podem ser predefinidas pelo serviço ou customizadas por você.
5. Quando um possível problema de segurança é sinalizado, a descoberta é encaminhada para o banco de dados Descobertas.
6. As descobertas são exibidas em seu painel de serviço no cartão **Activity Insights**.

</br>

## Coleta de dados
{: #activity-data}

O Activity Tracker coleta eventos que descrevem interações do usuário com relação às APIs do {{site.data.keyword.cloud_notm}}. É possível, então, armazenar os logs em um depósito do Object Storage para análise adicional.
{: shortdesc}

O Activity Tracker coleta eventos que descrevem interações do usuário com relação às APIs do {{site.data.keyword.cloud_notm}}.

As informações coletadas incluem:

* o endereço IP do inicializador da chamada API
* o usuário que foi autenticado
* o tipo de atividade
* O resultado da atividade
* E mais ...

Os dados brutos que são coletados são armazenados em um depósito do Cloud Object Storage, no qual é possível determinar o período de tempo em que ele é armazenado. Você possui os dados coletados e os controla, o que significa que você é responsável por armazená-lo, protegê-los e excluí-los. O {{site.data.keyword.security-advisor_short}}  mantém as descobertas por 90 dias. Durante esse tempo, os resultados são apresentados no cartão **Activity Insights** no painel de serviço. Portanto, embora você não veja mais a descoberta em seu painel após 90 dias, ainda é possível ter os dados brutos no armazenamento.

Do ponto de vista de segurança, geralmente é uma boa ideia limpar seus dados coletados quando os requisitos legais ou da empresa permitem que eles sejam excluídos. Para obter mais informações, confira [Excluindo objetos](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion).
{: tip}

## Activity Insights: Acesso
{: #ai-access}

O cartão do Activity Insights no painel de serviço resume qualquer indicação de atividade de conta inesperada ou alarmante por seus usuários e serviços. A atividade diferente do usual pode ser um mau comportamento pelos usuários ou serviços legítimos ou pode ser um sinal de que sua conta está comprometida. As descobertas que você vê no cartão são determinadas com base nos pacotes de regras padrão fornecidos pelo {{site.data.keyword.security-advisor_short}}.

O cartão introduz dois Indicadores-chaves de risco (KRIs):

* Identity and Access: descobertas que estão relacionadas aos serviços Identity and Access Management (IAM) ou ID do app.
* Dados e Kubernetes: descobertas que estão relacionadas a Key Protect, Kubernetes Service, Cloud Object Storage ou Certificate Manager.


## Entendendo pacotes de regras
{: #activity-packages}

Como um administrador de conta, é possível iniciar rapidamente o monitoramento de suas contas, aproveitando pacotes de regras.
{: shortdesc}

O serviço oferece pacotes de regras que estão associados a vários serviços, incluindo:

* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.Bluemix_notm}}  Identity and Access Management (IAM)
* {{site.data.keyword.cloudcerts_long_notm}}
* {{site.data.keyword.appid_long_notm}}
* {{site.data.keyword.keymanagementservicelong_notm}}
* {{site.data.keyword.cos_full_notm}}  (COS)

Se os pacotes atuais não atenderem a todas as suas necessidades, sempre será possível atualizar um pacote existente ou criar um novo para customizar as regras para a sua organização.

### O que é uma regra?
{: #ai-rule}

Uma regra é a combinação de condições e um único evento. É possível usar regras ou a combinação de regras para acionar descobertas que podem ser exibidas no painel do {{site.data.keyword.security-advisor_short}}.

Exemplo:

```
	{
		"comment": "Dormant Rule: Very high risk App ID activity... ",
		"dormant": true,
		"conditions": { 	… },
		"event": { … }
		"type": "aggregate"
	}
```
{: screen}

Além de uma condição e de um evento, as regras também podem conter os campos que estão localizados na tabela a seguir.

<table>
	<tr>
		<th colspan=2><img src="images/idea.png" alt="ícone de lâmpada"/> Entendendo os componentes de uma regra</th>
	</tr>
	<tr>
		<td><code> comment </code></td>
		<td>Sempre ignorado.</td>
	</tr>
	<tr>
		<td><code>dormant</code></td>
		<td>Um campo booleano que, se true, é ignorado. Se o valor for false ou indefinido, a regra será usada.</td>
	</tr>
	<tr>
		<td><code> type </code></td>
		<td>As opções incluem:  <code> aggregate </code>,  <code> coincident </code> e  <code> boolean </code>. Se o tipo não estiver designado a <code>aggregate</code> ou <code>coincident</code>, ele será avaliado como <code>boolean</code>.</td>
	</tr>
</table>

</br>

### O que é uma condição?
{: #ai-condition}

Uma condição básica é um bloco de construção que é composto por três componentes. Os blocos são ligados usando os operadores `any` e `all` e podem ser aninhados. Um operador `all` é o equivalente de `and` enquanto `any` é igual a `or`.

Exemplo:

```
	"condições": {
		"all": [ {
			"any": [ {
				"fact": "action", 				"operator": "equal", 				"value": "iam-groups.group.delete"
			},
			{
				"fact": "action", 				"operator": "equal", 				"value": "iam-groups.member.delete"
			}]
		}
	}
```
{: screen}

<table>
	<tr>
		<th colspan=2><img src="images/idea.png" alt="ícone de lâmpada"/> Entendendo os componentes de uma condição</th>
	</tr>
	<tr>
		<td><code> fato </code></td>
		<td>O evento CADF do Activity Tracker que está sendo inspecionado.</td>
	</tr>
	<tr>
		<td><code> operador </code></td>
		<td>As opções incluem: <code>equal</code>, <code>notEqual</code>, <code>lessThan</code>, <code>greaterThan</code>, <code>in</code> e <code>notIn</code></td>
	</tr>
	<tr>
		<td><code> value </code></td>
		<td>A maneira como uma ação é definida. O valor geralmente corresponde a uma chamada API que pode ser usada para interagir com o serviço.</td>
	</tr>
</table>

</br>

### O que é um evento?
{: #ai-event}

Um evento é composto por dois campos: `type` e `params.findingType`. O primeiro é um identificador exclusivo para uma regra, enquanto `params.findingType` é o nome da descoberta que é emitida para o serviço. O nome da descoberta permite que a descoberta seja exibida no painel do {{site.data.keyword.security-advisor_short}}.

Exemplo:

```
	{
		"conditions": { 	… },
		"event": {
			"type": "IKS high risk API",
			"params": {"findingType": "IKS-high-risk"}
		}
	}
```
{: screen}


### Tipo de regra: aggregate
{: #rule-aggregate}

Um tipo de regra agreggate conta o número de ocorrências de uma ação em um período de tempo específico e, em seguida, aciona uma descoberta se o limite definido for excedido. A regra é definida incluindo o limite e a janela de tempo para a condição booleana. Várias condições devem ser atendidas para que a regra seja definida.

* O tipo de regra deve ser  ` aggregate `.
* A condição raiz deve conter os fatos a seguir:

	```
	{
			"fact": "occurrences",
			"operator": [equal | greaterThan | greaterThanInclusive],
			"value": [positive number]
	},
	{
	    "fact": "withInLast", 	 "operator": "equal", 	 "value": "X [minutes|hours]", 	}
	```
	{: screen}

	Alguns esclarecimentos:
	* X = para um número inteiro positivo diferente de zero
	* Quando as horas são selecionadas, o valor máximo pode ser 24
	* Quando os minutos são selecionados, o valor máximo pode ser 1440.

** Exemplo **

O exemplo a seguir demonstra uma regra que conta cinco tentativas com falha em 30 minutos:

```
{
    "condições": {
        "all": [ {
                "fact": "action",
                "operator": "equal",
                "value": "iam-identity.user-apikey.login"
            },
            {
                "fact": "reason",
                "operator": "equal",
                "value": 400,
                "path": ".reasonCode"
            },
            {
                "fact": "occurrences",
                "operator": "equal",
                "value": 5
            },
            {
                "fact": "withInLast", "operator": "equal", "value": "30 minutes", }
        ]
    }, "event": {, "event": {
        "type": "failed-login-attempts",
        "params": {
            "findingType": "failed-login-attempts",
        }
    },
    "type" : "aggregate"
}
```
{: screen}

### Tipo de regra: coincidente
{: #rule-coincident}

Um tipo de regra coincident monitora ações para ver quantas vezes a mesma ação ocorre dentro de uma janela de tempo. A regra é definida pela inclusão de um espaço de tempo em um grupo de blocos de construção de condição básica. A ordem em que as ações ocorrem não tem significância na regra coincident, mas há várias condições que devem ser atendidas para que a regra seja definida.

* O tipo de regra deve ser  ` coincidente `.
* A condição raiz deve ser da variedade `all`.
* A condição raiz deve conter os fatos a seguir:

	```
	{
	    "fact": "actions",
	    "operator": "contains",
	    "value": [action value]
	},
	{
	    "fact": "withInLast", 	 "operator": "equal", 	 "value": "X [minutes|hours]", 	}
	```
	{: screen}

	Alguns esclarecimentos:
	* O valor `fact` deve ser plural: `actions`, não `action`.
	* X = para um número inteiro positivo diferente de zero
	* Quando as horas são selecionadas, o valor máximo pode ser 24
	* Quando os minutos são selecionados, o valor máximo pode ser 1440.


** Exemplo **

O exemplo a seguir demonstra uma regra que observa uma coincidência de três ações específicas que devem ocorrer dentro de um período de trinta minutos:

```
{
    "condições": {
        "all": [ {
                "fact": "actions",
                "operator": "contains",
                "value": "iam-identity.user-apikey.login"
            },
            {
                "fact": "actions",
                "operator": "contains",
                "value": "kms.secrets.list"
            },
            {
                "fact": "actions",
                "operator": "contains",
                "value": "kms.secrets.read"
            },
            {
                "fact": "withInLast", "operator": "equal", "value": "30 minutes", }
        ]
    }, "event": {, "event": {
        "type": "key-protect-keys-read",
        "params": {
            "findingType": "key-protect-keys-read",
        }
    },
    "type" : "coincident"
}
```
{: screen}

### Tipo de regra: boolean
{: #rule-boolean}

Uma regra booleana é composta de uma condição booleana e um evento. As regras booleanas são frequentemente usadas para monitorar o uso da API de alto risco, o uso da API que não se encaixa na janela de controle de mudança ou o uso da API por um inicializador que não está em uma lista de desbloqueio.

Se uma regra não for definida como `aggregate` ou `coincident`, ela será avaliada como uma regra `boolean`.

** Exemplo **

O exemplo a seguir demonstra uma regra que observa a exclusão da política fora da janela de controle de mudança por um usuário que não está na lista de desbloqueio:

```
{
	"condições": {
		"all": [ {
			"any": [ {
				"fact": "action", 				"operator": "equal", 				"value": "iam-groups.group.delete"
			},
			{
				"fact": "action", 				"operator": "equal", 				"value": "iam-groups.member.delete"
			}]
		},
		{
			"any": [ {
				"fact": "event_time",
				"operator": "lessThan",
				"value": "0800"
			},
			{
				"fact": "event_time",
				"operator": "greaterThan",
				"value": "1700"
			}
			]
		},
		{
			"fact": "initiator",
			"path": ".name",
			"operator": "notIn",
			"value": ["example@ibm.com", "example2@ibm.com"]
		}
		]
	}, "event": {, "event": {
		"type": "account-delete", "params": {
			"findingType": "iam-delete-account-threshold" }
	}
```
{: screen}

Deseja saber mais sobre regras booleanas? Confira os <a href="https://github.com/CacheControl/json-rules-engine/blob/master/docs/rules.md" target="_blank">docs CacheControl<img src="../../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.
{: tip}

## Próximas Etapas
{: #activity-next}

Pronto para iniciar? Efetue check-out  [ Ativando o Activity Insights ](/docs/services/security-advisor?topic=security-advisor-setup-activity)!
