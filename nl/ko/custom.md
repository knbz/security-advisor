---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-06"

keywords: Centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

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


# 사용자 정의 찾은 결과
{: #setup_custom}

{{site.data.keyword.security-advisor_short}}를 사용하면 오픈 소스, 사용자 정의 개발 또는 서드파티 서비스 등 기존 사용자 정의 보안 도구를 통합할 수 있습니다. 서드파티 찾은 결과를 통합하면 모든 보안 도구와 찾은 결과를 하나의 대시보드로 가져와서 중요한 보안 이벤트를 모니터할 수 있습니다.
{: shortdesc}


## 시작하기 전에
{: #custom-before-api}

서드파티 도구에서 찾은 결과를 통합하려면 먼저 다음 전제조건을 충족해야 합니다. 

1. 사용 중인 사용자 또는 서비스 ID에 **관리자** [IAM 역할](https://cloud.ibm.com/iam#/users)이 지정되어 있는지 확인하십시오.

2. {{site.data.keyword.cloud_notm}}에 로그인하십시오.

  ```
  ibmcloud login
  ```
  {: codeblock}

3. 계정 ID를 가져오십시오. 서비스 역할에 대한 자세한 정보는 [{{site.data.keyword.security-advisor_short}} 액세스 정책](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)을 확인하십시오.

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

4. IAM(Identity and Access Management) 토큰을 가져오십시오. 토큰은 각 API 요청의 `--header`에 사용됩니다.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  IAM 토큰은 60분마다 만료됩니다. API 키를 사용하여 [자동으로 새 토큰을 가져오는](/docs/iam?topic=iam-iamtoken_from_apikey) 방법에 대해 알아보려면 다음을 수행하십시오.
  {: tip}



## 찾은 결과 및 KRI 가져오기
{: #custom-adding}

API는 보안 도구 및 서비스로 보고되는 찾은 결과에 대한 중요 메타데이터를 저장하고, 조회하고, 검색하기 위해 아티팩트 메타데이터 스펙과 같은 Grafeas를 따릅니다. 통합은 API에서 주요 위험 지표(KRI)를 구성하여 수행할 수 있습니다.
{: shortdesc}


### 1단계: 새 찾은 결과 유형 등록
{: #custom-register-finding}

새 찾은 결과 유형을 등록하기 위해 노트를 작성할 수 있습니다. 노트를 작성하려면 [찾은 결과 API](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}를 사용하십시오. 사용자 정의 도구를 식별하기 위해 고유 제공자 ID를 선택해야 합니다. 프로세스를 자동화하는 경우 서비스 ID를 제공자 ID로 사용할 수 있습니다. 

요청 예제:

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{  \"kind\": \"FINDING\",  \"short_description\": \"My security tool finding\",  \"long_description\": \"Longer description of what the security tool found.\",  \"provder_id\": \"my-custom-tool\",  \"id\": \"my-custom-tool-findings-type\",  \"reported_by\": {    \"id\": \"my-custom-tool\",    \"title\": \"My custom security tool\"  } ,  \"finding\": {    \"severity\": \"MEDIUM\",    \"next_steps\": [      {        \"title\": \"Explain why it's reported as a risk.\"      }    ]  }}"
```
{: codeblock}

|일반사항 |설명 | 
|:-----------------|:-----------------|
| `note_name` | `<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type`  |
| `kind` | `FINDING` |
| `remediation` |문제를 해결하기 위해 수행되어야 하는 단계입니다. |
| `provider_id` |사용자 정의 보안 도구입니다. |
| `id` |보안 도구에서 발견된 찾은 결과 유형에 대한 ID입니다. |
{: class="simple-tab-table"}
{: caption="표 1. 명령의 일반 컴포넌트 이해" caption-side="top"}
{: #registerfindingtable1}
{: tab-title="General"}
{: tab-group="register"}

| 컨텍스트 |설명 | 
|:-----------------|:-----------------|
| `region` | 찾은 결과가 발생한 위치입니다. |
| `resource_id` | 리소스 ID입니다. |
| `resource_name` | 리소스 이름입니다. |
| `resource_type` | 리소스 유형입니다. |
| `service_name` | 서비스 이름입니다. |
{: class="simple-tab-table"}
{: caption="표 2. 명령의 컨텍스트 컴포넌트 이해" caption-side="top"}
{: #registerfinding2}
{: tab-title="Context"}
{: tab-group="register"}

|찾은 결과 |설명 | 
|:-----------------|:-----------------|
| `severity` |찾은 결과에서 제공되는 긴급성의 레벨입니다.  |
| `next_steps` |문제를 해결하기 위해 수행될 수 있는 단계입니다. |
| `url` |찾은 결과의 세부사항이 발견될 수 있는 URL입니다. |
{: class="simple-tab-table"}
{: caption="표 3. 명령의 찾은 결과 컴포넌트 이해" caption-side="top"}
{: #registerfinding3}
{: tab-title="Finding"}
{: tab-group="register"}

응답 예제:

```
  {
  "author": {
    "account_id": "account id",
      "email": "email id",
      "id": "IBM ID",
      "kind": "user"
  },
    "context": {
    "account_id": "account id"
  },
    "create_time": "2018-09-04T10:57:56.913871Z",
    "create_timestamp": 1536058676914,
    "finding": {
    "next_steps": [
        {
        "title": "Learn why this is reported as a risk"
        }
    ],
      "severity": "MEDIUM"
  },
    "id": "my-custom-tool-findings-type",
    "kind": "FINDING",
    "long_description": "See what my custom security tool found",
    "name": "<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
    "provider_id": "my-custom-tool",
    "provider_name": "<account id>/providers/my-custom-tool",
    "reported_by": {
    "id": "my-custom-tool",
      "title": "My Custom Security Tool"
  },
    "shared": true,
    "short_description": "My security tool finding",
    "update_time": "2018-09-04T10:57:56.913890Z",
    "update_timestamp": 1536058676914,
    "update_week_date": "2018-W36-2"
  }
```
{: screen}

응답의 파트로 리턴되는 노트의 이름을 기억하십시오. 이 예제에서 해당 값은 `/providers/my-custom-tool/notes/my-custom-tool-findings-type`입니다. 이 값은 다음 단계에서 사용됩니다.
{: tip}


### 2단계: 찾은 결과 게시
{: #custom-post-findings}

찾은 결과를 Security Advisor 대시보드에 KRI 또는 이벤트로 게시하려면 [발생](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence){: external}을 작성하십시오. 

각 카드에는 두 개의 KRI를 정의할 수 있습니다.
{: note}

요청 예제: 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/occurrences"  -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"note_name\": \"<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\",    \"kind\": \"FINDING\",    \"remediation\": \"How to resolve Data leakage threat\",    \"provider_id\": \"my-custom-tool\",    \"id\": \"my-custom-tool-finding-2\",    \"context\": {        \"region\": \"location\",        \"resource_id\": \"cluster crn\",        \"resource_name\": \"cloudkingdom\",        \"resource_type\": \"container\",        \"service_name\": \"kubernetes service\"    },    \"finding\": {        \"severity\": \"HIGH\",        \"next_steps\": [{            \"title\": \"Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities\",                        \"url\":\"https://cloud.ibm.com/containers-kubernetes/clusters\"        }],                \"short_description\": \"One of the pods in your cluster appears to be leaking an excessive amount of data\",                \"long_description\": \"One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod's normal behavior\"    }}"
```
{: codeblock}

|일반사항 |설명 | 
|:-----------------|:-----------------|
| `kind` | `FINDING` |
| `short_description` | 찾은 결과가 요약된 간략한 설명으로, 몇 개의 단어로만 구성됩니다. |
| `long_description` | 찾은 결과에 대한 세부사항이 포함된 자세한 설명입니다. |
| `provider_id` |사용자 정의 보안 도구입니다. |
| `id` |보안 도구에서 발견된 찾은 결과 유형에 대한 ID입니다. |
{: class="simple-tab-table"}
{: caption="표 2. 명령의 일반 컴포넌트 이해" caption-side="top"}
{: #postfinding1}
{: tab-title="General"}
{: tab-group="post"}

| 보고자 |설명 | 
|:-----------------|:-----------------|
| `id` |찾은 결과를 보고한 보안 도구의 ID입니다.  |
| `title` |찾은 결과를 보고한 보안 도구의 제목입니다. |
{: class="simple-tab-table"}
{: caption="표 2. 명령의 보고자 컴포넌트 이해" caption-side="top"}
{: #postfinding2}
{: tab-title="Reported by"}
{: tab-group="post"}

|찾은 결과 |설명 | 
|:-----------------|:-----------------|
| `severity` |찾은 결과에서 제공되는 긴급성의 레벨입니다.  |
| `next_steps` |문제를 해결하기 위해 수행될 수 있는 단계입니다. |
| `title` |찾은 결과의 제목입니다. |
{: class="simple-tab-table"}
{: caption="표 3. 명령의 찾은 결과 컴포넌트 이해" caption-side="top"}
{: #postfinding3}
{: tab-title="Finding"}
{: tab-group="post"}


응답 예제:

```
  {
  "author": {
    "account_id": "account id",
      "email": "email id ",
      "id": "user id",
      "kind": "user"
  },
    "context": {
    "account_id": "account id",
      "region": "location",
      "resource_id": "pluginId",
      "resource_name": "www.myapp.com",
      "resource_type": "worker",
      "service_name": "application"
  },
    "create_time": "2018-09-04T11:32:14.564809Z",
    "create_timestamp": 1536060734565,
    "finding": {
    "next_steps": [
        {
        "title": "Learn why this is reported as a risk",
          "url": "Details URL"
        }
    ],
      "severity": "HIGH"
  },
    "id": "my-custom-tool-finding-1",
    "kind": "FINDING",
    "long_description": "See what my custom security tool found",
    "name": "<account id>/providers/my-custom-tool/occurrences/my-custom-tool-finding-1",
    "note_name": "<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
    "provider_id": "my-custom-tool",
    "provider_name": "<account id>/providers/my-custom-tool",
    "remediation": "how to resolve this",
    "reported_by": {
    "id": "my-custom-tool",
      "title": "My Custom Security Tool"
  },
    "short_description": "My security tool finding",
    "update_time": "2018-09-04T11:32:14.564828Z",
    "update_timestamp": 1536060734565,
    "update_week_date": "2018-W36-2"
  }
```
{: screen}

### 3단계: 표시할 카드 정의
{: #custom-define-card}

[노트](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}를 작성하여 카드에서 찾은 결과를 대시보드에 표시하는 방식을 정의하십시오. 

요청 예제: 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"kind\": \"CARD\",        \"provider_id\": \"my-custom-tool\",    \"id\": \"custom-tool-card\",    \"short_description\": \"Security risk found by my custom tool\",        \"long_description\": \"More detailed description about why this security risk needs to be fixed\",    \"reported_by\": {      \"id\": \"my-custom-tool\",      \"title\": \"My security tool\"    },        \"card\": {      \"section\": \"My security tools\",      \"order\": 1 ,     \"title\": \"My security tool findings\",      \"subtitle\": \"My security tool\",      \"finding_note_names\": [        \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"      ],      \"elements\": [        {          \"kind\": \"NUMERIC\",          \"text\": \"Count of findings reported by my security tool\",          \"default_time_range\": \"1d\",          \"value_type\": {            \"kind\": \"FINDING_COUNT\",            \"finding_note_names\": [              \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"            ]          }        }      ]    }  }"
```
{: codeblock}

|일반사항 |설명 | 
|:-----------------|:-----------------|
| `provider_id` | 보안 도구의 ID입니다. |
| `id` |보안 도구에서 발견된 찾은 결과 유형에 대한 ID입니다. |
| `short_description` | 찾은 결과가 요약된 간략한 설명으로, 몇 개의 단어로만 구성됩니다. |
| `long_description` | 찾은 결과에 대한 세부사항이 포함된 자세한 설명입니다. |
{: class="simple-tab-table"}
{: caption="표 1. 명령의 일반 컴포넌트 이해" caption-side="top"}
{: #definecard1}
{: tab-title="General"}
{: tab-group="card"}

| 보고자 |설명 | 
|:-----------------|:-----------------|
| `id` |찾은 결과를 보고한 보안 도구의 ID입니다.  |
| `title` |찾은 결과를 보고한 보안 도구의 제목입니다. |
{: class="simple-tab-table"}
{: caption="표 2. 명령의 보고자 컴포넌트 이해" caption-side="top"}
{: #definecard2}
{: tab-title="Reported by"}
{: tab-group="card"}

|카드 |설명 | 
|:-----------------|:-----------------|
| `section` | 카드를 표시할 섹션입니다. 각 섹션에는 6개의 카드가 포함된 최대 3개의 사용자 정의 섹션이 있을 수 있습니다. 최대 문자 수: 30 |
| 선택사항: `order` | 지정된 섹션에서 카드가 표시되는 순서입니다. 순서는 1 - 6 범위로 지정됩니다. 다른 카드에 이미 적용된 숫자를 선택할 경우 작성이 실패합니다. "제공한 순서는 이미 섹션의 다른 카드에 사용되었습니다."라는 오류 메시지가 표시됩니다. 제공한 순서가 현재 카드 수 +1보다 클 경우 카드 작성이 실패합니다. 예를 들어 현재 두 개의 카드가 있는 상태에서 다른 카드를 작성할 경우, 총 3개의 카드가 있으므로 카드 순서에 5를 지정할 수 없습니다. 카드 순서를 지정하지 않을 경우 카드가 지정된 섹션에 알파벳순으로 배열됩니다. |
| `title` | 카드에 지정할 제목입니다. 최대 문자 수: 28 |
| `subtitle` | 카드에 지정할 부제입니다. 최대 문자 수: 30 |
| `finding_note_names` | `providers//notes/my-custom-tool-findings-type` |
{: class="simple-tab-table"}
{: caption="표 3. 명령의 카드 컴포넌트 이해" caption-side="top"}
{: #definecard3}
{: tab-title="Card"}
{: tab-group="card"}

| 요소: 값 유형 |설명 | 
|:-----------------|:-----------------|
| `kind` |옵션에는 `NUMERIC`, `TIME_SERIES`, `BREAKDOWN`이 있습니다. |
| `text` | 표시할 텍스트입니다. kind가 `NUMERIC`일 경우 최대 문자 수는 60입니다. kind가 `TIME_SERIES` 또는 `BREAKDOWN`일 경우, 최대 문자 수는 65입니다. |
| `default_time_range` |확인할 시간입니다. 값은 일 수로 설정됩니다. 현재 옵션에는 `1d`, `2d`, `3d`, `4d`가 있습니다. |
| `value_type` | 요소의 종류입니다. kind가 `NUMERIC`일 경우 필드는 `value_type`이며 카드당 최대 4개의 요소를 포함할 수 있습니다. kind가 `TIME_SERIES` 또는 `BREAKDOWN`일 경우 필드는 `value_types`입니다. `TIME_SERIES` 또는 `BREAKDOWN`의 최대 개수는 1입니다. 숫자 항목만 있을 경우 카드당 최대 4개의 요소가 있을 수 있습니다. 조합을 사용하려는 경우 최대 두 개의 숫자 항목과 time series 또는 breakdown 중 하나를 포함할 수 있습니다. time series와 breakdown이 동일한 카드에 포함될 수는 없습니다. 값 유형을 time series에 대한 배열로 정의한 경우 최대 세 개의 항목을 포함할 수 있습니다. |
| `value_type`: `kind` | 값 유형입니다. 옵션에는 `KRI` 및 `FINDING_COUNT`가 있습니다. |
| `value_type`: `finding_note_names` | `kind`가 `FINDING_COUNT`일 경우, 카드에 표시하려는 찾은 결과의 이름이며, 배열로 지정됩니다. |
| `value_type`: `kri_note_names` | `kind`가 `FINDING_COUNT`일 경우, 카드에 표시하려는 찾은 결과의 이름이며, 배열로 지정됩니다. |
| `value_type`: `text` | 요소 유형의 텍스트입니다. 최대 문자 수는 22입니다. |
{: class="simple-tab-table"}
{: caption="표 4. 명령의 요소 컴포넌트 이해" caption-side="top"}
{: #definecard4}
{: tab-title="Elements"}
{: tab-group="card"}

응답 예제:

```
{
"author": {
  "account_id": "account id",
      "email": "email id ",
      "id": "user id",
      "kind": "user"
},
    "card": {
  "elements": [
        {
      "default_time_range": "1d",
          "kind": "NUMERIC",
          "text": "Count of findings reported by my security tool",
          "value_type": {
        "finding_note_names": [
          "providers/my-custom-tool/notes/my-custom-tool-findings-type"
        ],
            "kind": "FINDING_COUNT"
          }
    }
  ],
      "finding_note_names": [
    "providers/my-custom-tool/notes/my-custom-tool-findings-type"
  ],
  "section": "My Security Tools",
  "order": "1",
  "title": "My Security Tool Findings",
  "subtitle": "My Security Tool",
},
"context": {
  "account_id": "<account id>"
},
    "create_time": "2018-09-04T11:49:36.056047Z",
    "create_timestamp": 1536061776056,
    "id": "custom-tool-card",
    "kind": "CARD",
    "long_description": "Details about why this is security risk to be fixed",
    "name": "<account id>/providers/my-custom-tool/notes/custom-tool-card",
    "provider_id": "my-custom-tool",
    "provider_name": "<account id>/providers/my-custom-tool",
    "reported_by": {
  "id": "my-custom-tool",
      "title": "My Security Tool"
},
    "shared": true,
    "short_description": "security risk found by my custom tool",
    "update_time": "2018-09-04T11:49:36.056066Z",
    "update_timestamp": 1536061776056,
    "update_week_date": "2018-W36-2"
  }
```
{: screen}


## 사용법 예제
{: #custom-example}

이름이 `cloudkingdom`인 {{site.data.keyword.containershort_notm}} 클러스터가 실행되는 애플리케이션이 있다고 응답하십시오. 애플리케이션의 크기에 따라 모두 동시에 모니터해야 하는 여러 개의 팟(Pod)이 클러스터 내에 있을 수 있습니다. 다양한 위협에 대해 클러스터를 모니터하고 발견하는 여러 개의 사용자 정의 도구가 있는 경우에는 어떨까요? 클러스터에 있는 팟(Pod) 중 하나가 비정상적인 양의 데이터를 외부 서버로 보내기 시작하면 가능한 한 빨리 알고자 할 것입니다. 데이터 전송을 모니터하는 사용자 정의 도구는 찾은 결과를 발견하여 이를 {{site.data.keyword.security-advisor_short}}로 전송할 수 있습니다. 문제를 발견하는 다른 사용자 정의 통합이 있으면 사용자 정의 통합도 찾은 결과를 {{site.data.keyword.security-advisor_short}}로 전송할 수 있습니다. 그런 다음 {{site.data.keyword.security-advisor_short}}는 모든 사용자 모니터링 도구에서 찾은 결과를 단일 대시보드에 표시합니다. 대시보드에서 모든 경보의 개요를 빠르게 확인하고 문제를 조사하고 조치방안 단계를 수행하는 방법에 대해 알아볼 수 있습니다.

페이로드 예제:

```
{
	"note_name": "<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
	"kind": "FINDING",
	"remediation": "How to resolve Data leakage threat",
	"provider_id": "my-custom-tool",
	"id": "my-custom-tool-finding-2",
	"context": {
		"region": "location",
		"resource_id": "cluster crn",
		"resource_name": "cloudkingdom",
		"resource_type": "container",
		"service_name": "kubernetes service"
	},
	"finding": {
		"severity": "HIGH",
		"next_steps": [{
			"title": "Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities",
                        "url":"https://cloud.ibm.com/containers-kubernetes/clusters"
		}],
                "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
                "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod’s normal behavior"
	}
}
```
{: screen}
