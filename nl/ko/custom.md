---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-13"

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


# 사용자 정의
{: #setup_custom}

{{site.data.keyword.security-advisor_short}}를 사용하면 오픈 소스, 사용자 정의 개발 또는 서드파티 서비스 등 기존 사용자 정의 보안 도구를 통합할 수 있습니다. 통합 목록에 URL을 추가하여 {{site.data.keyword.security-advisor_short}}에서 다른 도구로 직접 연결을 생성할 수 있습니다. 또한 API를 사용하여 API 및 주요 위험 지표(KRI)를 구성함으로써 중요한 보안 이벤트를 대시보드의 새 카드로 직접 가져오도록 서드파티가 찾은 결과를 통합할 수 있습니다.
{: shortdesc}


## 직접 연결 추가
{: #setup-custom-gui}

{{site.data.keyword.security-advisor_short}} 대시보드를 사용하여 보안 도구를 쉽게 추적할 수 있습니다.
{: shortdesc}

### 시작하기 전에
{: #custom-before-gui}

통합을 추가하기 전에 통합하려는 파트너에 대한 계정이 있어야 합니다.

{{site.data.keyword.security-advisor_short}}는 파트너 서비스와 관련된 어떤 인증 정보도 지속시키지 않습니다. 엔터프라이즈 사용자는 SAML을 사용하여 {{site.data.keyword.Bluemix_notm}} 및 비즈니스 파트너에 모두 인증해야 합니다.
{: note}

### 연결 구성
{: #custom-configure-connection}

1. 보안 도구에 로그인한 후 고유 URL을 가져오십시오.

2. 콘솔을 사용하여 {{site.data.keyword.cloud_notm}}에 로그인하십시오.

3. **사용자 정의 통합 > 직접 연결**을 클릭하십시오. 화면이 표시됩니다.

  1. 솔루션 이름을 제공하십시오. 이름에 영숫자 문자, 공백 및 대시(-)만 사용할 수 있습니다.

  2. `www.<website>.<domain>` 형식으로 솔루션의 URL을 입력하십시오.

  3. 도구를 나타내는 아이콘 또는 이미지를 업로드하십시오.

  4. **연결**을 클릭하여 구성을 완료하십시오. {{site.data.keyword.security-advisor_short}}는 통합에 필요한 아티팩트(예: 서비스 ID, API 키, 계정 ID 및 메타데이터)를 작성합니다. `작성자` 역할이 지정됩니다.



## 서드파티 찾은 결과 통합

API는 보안 도구 및 서비스로 보고되는 찾은 결과에 대한 중요 메타데이터를 저장하고, 조회하고, 검색하기 위해 아티팩트 메타데이터 스펙과 같은 Grafeas를 따릅니다.
{: #setup-custom-api}

### 시작하기 전에
{: #custom-before-api}

서드파티 도구에서 찾은 결과를 통합하려면 다음 전제조건을 충족해야 합니다.

1. 사용 중인 사용자 또는 서비스 ID에 **관리자** [IAM 역할](https://cloud.ibm.com/iam#/users)이 지정되어 있는지 확인하십시오.

1. {{site.data.keyword.cloud_notm}}에 로그인하십시오.

  ```
  ibmcloud login
  ```
  {: codeblock}

2. 계정 ID를 가져오십시오. 서비스 역할에 대한 자세한 정보는 [{{site.data.keyword.security-advisor_short}} 액세스 정책](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)을 확인하십시오.

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

3. IAM(Identity and Access Management) 토큰을 가져오십시오. 토큰은 각 API 요청의 `--header`에 사용됩니다.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  IAM 토큰은 60분마다 만료됩니다. API 키를 사용하여 [자동으로 새 토큰을 가져오는](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey) 방법에 대해 알아보려면 다음을 수행하십시오.
  {: tip}



### 찾은 결과 및 KRI 가져오기
{: #custom-adding}

1. 참고를 작성하여 새로운 유형의 찾은 결과를 등록하십시오. 참고를 작성하려면 [찾은 결과 API](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote)를 사용하십시오. 사용자 정의 도구를 식별하기 위해 고유 제공자 ID를 선택해야 합니다. 서비스 ID를 사용하여 프로세스를 자동화하는 경우 서비스 ID를 제공자 ID로 사용할 수 있습니다.

  요청 예제:

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Content-Type: application/json" -d "{ \"kind\": \"FINDING\", \"short_description\": \"My security tool finding\", \"long_description\": \"See what my custom security tool found\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-findings-type\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Custom Security Tool\" }, \"finding\": { \"severity\": \"MEDIUM\", \"next_steps\": [ { \"title\": \"Learn why this is reported as a risk\" } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="자세한 정보 아이콘"/> 이 명령 컴포넌트 이해 </th>
    </thead>
    <tbody>
      <tr>
        <td><code>note_name</code></td>
        <td><code>&lt;account id&gt;/providers/my-custom-tool/notes/my-custom-tool-findings-type</code></td>
      </tr>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>remediation</code></td>
        <td>문제를 해결하기 위해 수행되어야 하는 단계입니다.</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>사용자 정의 보안 도구입니다.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>보안 도구에서 발견된 찾은 결과 유형에 대한 ID입니다.</td>
      </tr>
      <tr>
        <td><code>context</code><ul><li><code>region</code></li><li><code>resource_id</code></li> <li><code>resource_name</code></li> <li><code>resource_type</code></li> <li><code>service_name</code></li></ul></td>
        <td></br><ul><li><code>찾은 결과가 발생된 위치입니다.</code></li><li><code>특정 리소스의 ID입니다.</code></li> <li><code>리소스의 이름입니다.</code></li> <li><code>리소스의 유형입니다.</code></li> <li><code>서비스의 이름입니다.</code></li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>url</code></li></ul></td>
        <td></br><ul><li>찾은 결과에서 제공되는 긴급성의 레벨입니다.</li> <li>문제를 해결하기 위해 수행될 수 있는 단계입니다.</li> <li>찾은 결과의 세부사항이 발견될 수 있는 URL입니다.</li></ul></td>
      </tr>
    </tbody>
  </table>

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

2. KRI 또는 이벤트로 게시, 그 외의 경우 [발생](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence)으로 알려져 있음.

  각 카드에는 두 개의 KRI를 정의할 수 있습니다.
  {: note}

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account-id>/providers/my-custom-tool/occurrences" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Replace-If-Exists: true" -H "Content-Type: application/json" -d "{ \"note_name\": \"<account-id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\", \"kind\": \"FINDING\", \"remediation\": \"how to resolve this\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-finding-1\", \"context\": { \"region\": \"location\", \"resource_id\": \"pluginId\", \"resource_name\": \"www.myapp.com\", \"resource_type\": \"worker\", \"service_name\": \"application\" }, \"finding\": { \"severity\": \"HIGH\", \"next_steps\": [{ \"url\": \"Details URL\" }] }}"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="자세한 정보 아이콘"/> 이 명령 컴포넌트 이해 </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>찾은 결과가 요약된 짧은 설명으로 몇 개의 단어로만 구성됩니다.</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>찾은 결과에 대한 세부사항이 포함된 긴 설명입니다.</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>사용자 정의 보안 도구입니다.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>보안 도구에서 발견된 찾은 결과 유형에 대한 ID입니다.</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>찾은 결과를 보고한 보안 도구의 ID입니다.</li><li>찾은 결과를 보고한 보안 도구의 제목입니다.</li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>title</code></li></ul></td>
        <td></br><ul><li>찾은 결과에서 제공되는 긴급성의 레벨입니다.</li> <li>문제를 해결하기 위해 수행될 수 있는 단계입니다.</li> <li>찾은 결과의 제목입니다.</li></ul></td>
      </tr>
    </tbody>
  </table>

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

3. [참고](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote)를 작성하여 찾은 결과를 표시하려면 대시보드의 카드를 정의하십시오.

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam token>" -H "Content-Type: application/json" -d "{ \"kind\": \"CARD\", \"provider_id\": \"my-custom-tool\", \"id\": \"custom-tool-card\", \"short_description\": \"security risk found by my custom tool\", \"long_description\": \"Details about why this is security risk to be fixed\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Security Tool\" }, \"card\": { \"section\": \"My Security Tools\", \"title\": \"My Security Tool Findings\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ], \"elements\": [ { \"kind\": \"NUMERIC\", \"text\": \"Count of findings reported by my security tool\", \"default_time_range\": \"1d\", \"value_type\": { \"kind\": \"FINDING_COUNT\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ] } } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="자세한 정보 아이콘"/> 이 명령 컴포넌트 이해 </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>CARD</code></td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>사용자 정의 보안 도구입니다.</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>보안 도구에서 발견된 찾은 결과 유형에 대한 ID입니다.</td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>찾은 결과가 요약된 설명으로 몇 개의 단어로만 구성됩니다.</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>찾은 결과에 대한 세부사항이 포함된 긴 설명입니다.</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>찾은 결과를 보고한 보안 도구의 ID입니다.</li><li>찾은 결과를 보고한 보안 도구의 제목입니다.</li></ul></td>
      </tr>
      <tr>
        <td><code>card</code> <ul><li><code>section</code></li> <li><code>title</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>카드에 적합한 섹션입니다.</li> <li>카드의 제목입니다.</li> <li><code>providers/<provider_id>/notes/my-custom-tool-findings-type</code></li></ul></td>
      </tr>
      <tr>
        <td><code>elements</code> <ul><li><code>kind</code></li> <li><code>text</code></li> <li><code>default_time_range</code></li></ul></td>
        <td></br><ul><li>요소의 유형입니다.</li> <li>표시할 텍스트입니다.</li> <li>확인할 시간입니다.</li></ul></td>
      </tr>
      <tr>
        <td><code>value_type</code> <ul><li><code>kind</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>값의 유형입니다.</li> <li>카드에서 확인할 찾은 결과의 이름입니다.</li></ul></td>
      </tr>
    </tbody>
  </table>

  응답 예제:
  ```
    {
    "author": {
      "account_id": "<account id",
      "email": "email id",
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
      "title": "My Security Tool Findings"
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

4. 작성한 카드를 보려면 서비스 대시보드로 이동하십시오.

## 사용법 예제
{: #custom-example}

이름이 `cloudkingdom`인 {{site.data.keyword.containershort_notm}} 클러스터가 실행되는 애플리케이션이 있다고 응답하십시오. 애플리케이션의 크기에 따라 클러스터 내에 여러 개의 팟(Pod)이 있어 동시에 모두 모니터할 수 있습니다. 다양한 위협에 대해 클러스터를 모니터하고 발견하는 여러 개의 사용자 정의 도구가 있는 경우에는 어떨까요? 클러스터에 있는 팟(Pod) 중 하나가 비정상적인 양의 데이터를 외부 서버로 보내기 시작하면 가능한 한 빨리 알고자 할 것입니다. 데이터 전송을 모니터하는 사용자 정의 도구는 찾은 결과를 발견하여 이를 {{site.data.keyword.security-advisor_short}}로 전송할 수 있습니다. 문제를 발견하는 다른 사용자 정의 통합이 있으면 사용자 정의 통합도 찾은 결과를 {{site.data.keyword.security-advisor_short}}로 전송할 수 있습니다. 그런 다음 {{site.data.keyword.security-advisor_short}}는 모든 사용자 모니터링 도구에서 찾은 결과를 단일 대시보드에 표시합니다. 대시보드에서 모든 경보의 개요를 빠르게 확인하고 문제를 조사하고 조치방안 단계를 수행하는 방법에 대해 알아볼 수 있습니다.


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
                        "url":"https://console.bluemix.net/containers-kubernetes/clusters"
		}],
                "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
                "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod’s normal behavior"
	}
}
```
{: screen}
