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

# Activity Insights(미리보기)
{: #activity}

{{site.data.keyword.security-advisor_long}}를 사용하면 {{site.data.keyword.cloud_notm}} Activity Tracker를 사용하여 {{site.data.keyword.Bluemix_notm}} 계정에서 의심스러운 사용자 활동을 발견할 수 있습니다.
{: shortdesc}


## 작동 방법
{: #activity-how}

Activity Insights 기능은 {{site.data.keyword.security-advisor_short}} 서비스에 대한 추가 기능입니다. 이 기능이 구성되고 사용으로 설정되어 있는 경우 사용자 동작을 로그하고 분석하여 규칙을 기반으로 의심스러운 활동을 식별합니다. 기본 규칙을 사용하거나 조직에 맞는 사용자 정의 규칙을 작성할 수 있습니다.

정보의 플로우를 확인하려면 다음 이미지를 참조하십시오.

![Activity Insights 플로우 다이어그램](images/activity-insights-flow.png)

1. 계정 관리자는 Activity Insights를 클러스터에 설치할 수 있습니다.
2. 추가 기능을 하나의 클러스터에 설치하면 전체 계정에 대한 활동 추적 트래커 로그를 모니터할 수 있습니다.
3. 활동 로그는 Cloud Object Storage 버킷으로 전달되어 삭제할 때까지 저장됩니다. {{site.data.keyword.security-advisor_short}} GUI를 사용하여 버켓을 작성하면 서비스에서 로그를 볼 수 있도록 서비스 IAM 역할이 지정됩니다.
4. Activity Insights가 사용으로 설정되면 서비스에서 사전 정의하거나 사용자가 사용자 정의할 수 있는 규칙에 따라 COS 버킷의 원시 데이터가 분석됩니다.
5. 가능한 보안 문제를 플래그 지정하면 찾은 결과가 찾은 결과 데이터베이스로 전달됩니다.
6. 찾은 결과는 **Activity Insights** 카드로 서비스 대시보드에 표시됩니다.

</br>

## 데이터 수집
{: #activity-data}

Activity Tracker는 {{site.data.keyword.cloud_notm}} API에 대한 사용자 상호작용을 설명하는 이벤트를 수집합니다. 그런 다음 추가 분석을 위해 로그를 Object Storage 버킷에 저장할 수 있습니다.
{: shortdesc}

Activity Tracker는 {{site.data.keyword.cloud_notm}} API에 대한 사용자 상호작용을 설명하는 이벤트를 수집합니다. 

수집된 정보는 다음과 같습니다.

* API 호출 개시자의 IP 주소
* 인증된 사용자
* 활동 유형
* 활동 결과
* 기타...

수집된 원시 데이터는 저장되는 기간을 판별할 수 있는 Cloud Object Storage 버킷에 저장됩니다. 수집한 데이터를 소유하고 관리한다는 것은 데이터를 저장하고 보호하고 삭제할 책임이 있음을 의미합니다. {{site.data.keyword.security-advisor_short}}는 90일 동안 찾은 결과를 유지보수합니다. 그 기간 동안 결과는 서비스 대시보드의 **Activity Insights** 카드에 표시됩니다. 따라서 90일 후 대시보드에 더 이상 찾은 결과가 표시되지 않지만 원시 데이터는 스토리지에 여전히 있을 수 있습니다.

보안 관점에서 법적 또는 회사 요구사항에 따라 삭제할 수 있으면 일반적으로 수집된 데이터는 제거하는 것이 좋습니다. 자세한 정보는 [오브젝트 삭제](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion)를 참조하십시오.
{: tip}

## Activity Insights: 액세스
{: #ai-access}

서비스 대시보드의 Activity Insights 카드에는 사용자 및 서비스별 예기치 못한 또는 알람 계정 활동이 요약됩니다. 일반적이지 않은 활동은 인증된 사용자 및 서비스에 의한 오동작일 수 있으며 계정이 손상되었다는 사인일 수도 있습니다. 카드에 표시되는 찾은 결과는 기본 규칙 패키지에서 제공하는 {{site.data.keyword.security-advisor_short}}를 기반으로 판별됩니다.

카드에는 두 가지 주요 위험 지표(KRI)가 있습니다.

* ID 및 액세스: ID 및 액세스 관리(IAM) 또는 앱 ID 서비스와 관련된 찾은 결과.
* 데이터 및 Kubernetes: 키 보호, Kubernetes 서비스, Cloud Object Storage 또는 Certificate Manager와 관련된 찾은 결과.


## 규칙 패키지 이해
{: #activity-packages}

계정 관리자는 규칙 패키지를 활용하여 계정 모니터링을 신속하게 시작할 수 있습니다.
{: shortdesc}

이 서비스는 다음과 같은 여러 서비스와 연관된 규칙 패키지를 제공합니다.

* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.Bluemix_notm}} Identity and Access Management(IAM)
* {{site.data.keyword.cloudcerts_long_notm}}
* {{site.data.keyword.appid_long_notm}}
* {{site.data.keyword.keymanagementservicelong_notm}}
* {{site.data.keyword.cos_full_notm}}(COS)

현재 패키지가 모든 요구사항을 충족하지 못하면 기존 패키지를 언제든지 업데이트하거나 새로운 패키지를 작성하여 규칙을 조직에 맞게 사용자 조정할 수 있습니다.

### 규칙은 무엇입니까?
{: #ai-rule}

규칙은 조건과 단일 이벤트의 조합입니다. 규칙 또는 규칙 조합을 사용하여 {{site.data.keyword.security-advisor_short}} 대시보드에 표시할 수 있는 찾은 결과를 트리거할 수 있습니다. 

예를 들면, 다음과 같습니다.

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

규칙에는 조건 및 이벤트 외에 다음 표에 있는 필드도 포함될 수 있습니다.

<table>
	<tr>
		<th colspan=2><img src="images/idea.png" alt="전구 아이콘"/> 규칙의 컴포넌트 이해</th>
	</tr>
	<tr>
		<td><code>comment</code></td>
		<td>항상 무시합니다.</td>
	</tr>
	<tr>
		<td><code>dormant</code></td>
		<td>true인 경우 부울 필드는 무시됩니다. 값이 false이거나 정의되지 않으면 규칙이 사용됩니다.</td>
	</tr>
	<tr>
		<td><code>type</code></td>
		<td>옵션으로는 <code>aggregate</code>, <code>coincident</code> 및 <code>boolean</code>이 있습니다. 유형이 <code>aggregate</code> 또는 <code>coincident</code>로 지정되지 않으면 <code>beelean</code>으로 평가됩니다.</td>
	</tr>
</table>

</br>

### 조건은 무엇입니까?
{: #ai-condition}

기본 조건은 세 가지 컴포넌트로 구성된 구성 요소입니다. 블록은 `any` 및 `all` 연산자를 사용하여 바인딩되며 중첩될 수 있습니다. `all` 연산자는 `and`에 해당하는 반면 `any` 연산자는 `or`와 동일합니다.

예를 들면, 다음과 같습니다.

```
	"conditions": {
		"all": [{
			"any": [{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.group.delete"
			},
			{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.member.delete"
			}]
		}
	}
```
{: screen}

<table>
	<tr>
		<th colspan=2><img src="images/idea.png" alt="전구 아이콘"/> 조건의 컴포넌트 이해</th>
	</tr>
	<tr>
		<td><code>fact</code></td>
		<td>검사 중인 Activity Tracker CADF 이벤트입니다.</td>
	</tr>
	<tr>
		<td><code>operator</code>
</td>
		<td>포함되는 옵션: <code>equal</code>, <code>notEqual</code>, <code>lessThan</code>, <code>greaterThan</code>, <code>in</code> 및 <code>notIn</code></td>
	</tr>
	<tr>
		<td><code>value</code></td>
		<td>조치가 정의되는 방법입니다. 값은 일반적으로 서비스와 상호작용하는 데 사용할 수 있는 API 호출에 해당합니다.</td>
	</tr>
</table>

</br>

### 이벤트란 무엇입니까?
{: #ai-event}

이벤트는 2개의 필드(`type` 및 `params.findingType`)로 구성됩니다. type은 규칙의 고유 ID이며 `params.findingType`은 서비스로 발행될 찾은 결과의 이름입니다. 찾은 결과 이름을 사용하면 찾은 결과를 {{site.data.keyword.security-advisor_short}} 대시보드에 표시할 수 있습니다.

예를 들면, 다음과 같습니다.

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


### 규칙 유형: 집계
{: #rule-aggregate}

집계 규칙 유형은 특정 시간 범위에서 조치 발생 횟수를 계산한 다음 정의된 임계값을 초과하면 찾은 결과를 트리거합니다. 규칙은 임계값과 시간 창을 부울 조건에 추가하여 정의됩니다. 규칙을 정의하려면 몇 가지 조건이 충족되어야 합니다.

* 규칙 유형이 `aggregate`이어야 합니다.
* 루트 조건에 다음 팩트가 포함되어야 합니다.

	```
	{
			"fact": "occurrences",
			"operator": [equal | greaterThan | greaterThanInclusive],
			"value": [positive number]
	},
	{
	    "fact": "withInLast",
	    "operator": "equal",
	    "value": "X [minutes|hours]",
	}
	```
	{: screen}

	몇 가지 설명:
	* X = 0이 아닌 양의 정수로
	* 시간을 선택하는 경우 최대값은 24임
	* 분을 선택하는 경우 최대값은 1440임

**예제**

다음 예제는 30분 내에 5회 실패한 시도를 계산하는 규칙을 표시합니다.

```
{
    "conditions": {
        "all": [
            {
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
                "fact": "withInLast",
                "operator": "equal",
                "value": "30 minutes",
            }
        ]
    },
    "event": {
        "type": "failed-login-attempts",
        "params": {
            "findingType": "failed-login-attempts",
        }
    },
    "type" : "aggregate"
}
```
{: screen}

### 규칙 유형: 일치
{: #rule-coincident}

일치 규칙 유형은 조치를 모니터하여 동일한 조치가 시간 창에서 몇 번 발생하는지 확인합니다. 규칙은 기본 조건 구성 요소 그룹에 시간 창을 추가하여 정의합니다. 조치가 발생하는 순서는 일치 규칙에서 의미가 없지만 규칙을 정의하려면 몇 가지 조건이 충족되어야 합니다.

* 규칙 유형이 `coincident`이어야 합니다.
* 루트 조건은 `all` 종류이어야 합니다.
* 루트 조건에 다음 팩트가 포함되어야 합니다.

	```
	{
	    "fact": "actions",
	    "operator": "contains",
	    "value": [action value]
	},
	{
	    "fact": "withInLast",
	    "operator": "equal",
	    "value": "X [minutes|hours]",
	}
	```
	{: screen}

	몇 가지 설명:
	* `fact` 값은 복수여야 합니다. `action`이 아닌 `actions`이어야 합니다.
	* X = 0이 아닌 양의 정수로
	* 시간을 선택하는 경우 최대값은 24임
	* 분을 선택하는 경우 최대값은 1440임


**예제**

다음 예제는 30분 내에 발생해야 하는 세 가지 특정 조치의 동시 발생을 감시하는 규칙을 보여줍니다.

```
{
    "conditions": {
        "all": [
            {
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
                "fact": "withInLast",
                "operator": "equal",
                "value": "30 minutes",
            }
        ]
    },
    "event": {
        "type": "key-protect-keys-read",
        "params": {
            "findingType": "key-protect-keys-read",
        }
    },
    "type" : "coincident"
}
```
{: screen}

### 규칙 유형: 부울
{: #rule-boolean}

부울 규칙은 부울 조건과 이벤트로 구성됩니다. 부울 규칙은 고위험의 API 사용, 변경 제어 창 외부에 있는 API 사용 또는 허용 목록에 없는 개시자의 API 사용을 모니터하는 데 자주 사용됩니다.

규칙이 `aggregate` 또는 `coincident`로 정의되지 않으면 `boolean` 규칙으로 평가됩니다.

**예제**

다음 예제는 허용 목록에 없는 사용자에 의한 변경 제어 창 외부에서의 정책 삭제를 감시하는 규칙을 보여줍니다.

```
{
	"conditions": {
		"all": [{
			"any": [{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.group.delete"
			},
			{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.member.delete"
			}]
		},
		{
			"any": [{
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
	},
	"event": {
		"type": "account-delete",
		"params": {
			"findingType": "iam-delete-account-threshold"
		}
	}
```
{: screen}

부울 규칙에 대한 자세한 정보를 확인하시겠습니까? <a href="https://github.com/CacheControl/json-rules-engine/blob/master/docs/rules.md" target="_blank">CacheControl 문서 <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오.
{: tip}

## 다음 단계
{: #activity-next}

시작할 준비가 되셨습니까? [Activity Insights 사용](/docs/services/security-advisor?topic=security-advisor-setup-activity)을 참조하십시오!
