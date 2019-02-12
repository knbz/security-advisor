---

copyright:
  years: 2018
lastupdated: "2018-11-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# 고유한 보안 도구 통합
{: #custom}

{{site.data.keyword.security-advisor_long}}는 IBM Cloud에서 보안 관리를 통합하고 개선하는 데 사용할 수 있는 찾은 결과 API를 제공합니다. 찾은 결과 API를 사용하여 파트너 서비스 및 사용자 정의 보안 도구에 대한 새 찾은 결과 유형을 등록할 수 있습니다. 메타데이터가 등록된 후 도구는 API를 사용하여 KPI 및 이벤트로서 찾은 결과의 발생을 전송할 수 있습니다. 찾은 결과는 개별 카드로 {{site.data.keyword.security-advisor_short}} 대시보드에 표시됩니다.
{: shortdesc}

{{site.data.keyword.security-advisor_short}}는 모든 보안 도구 및 서비스로 보고되는 찾은 결과에 대한 중요 메타데이터를 작성하고, 조회하고, 검색하기 위해 아티팩트 메타데이터 API 스펙과 같은 [Grafeas](https://grafeas.io/)를 따릅니다.

## 고유한 도구 통합
{: #integrate}

**시작하기 전에**

1. {{site.data.keyword.Bluemix_notm}}에 로그인하십시오.

  ```
  ibmcloud login
  ```
  {: codeblock}

2. 계정 ID를 가져오십시오. ID가 **관리자** [IAM 역할](https://console.bluemix.net/iam/#/users)에 지정되는지 확인하십시오. 서비스 역할에 대한 자세한 정보는 [{{site.data.keyword.security-advisor_short}} 액세스 정책](/docs/services/security-advisor/iam.html)을 확인하십시오.

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

3. IAM 토큰을 가져오십시오. 토큰은 각 API 요청의 `--header`에 사용됩니다.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

</br>

**찾은 결과 추가 및 모니터링**

1. 참고를 작성하여 새로운 유형의 찾은 결과를 등록하십시오. 참고를 작성하려면 [찾은 결과 API](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote)를 사용하십시오.

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
  {: codeblock}

2. [발생](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence)을 게시하여 찾은 결과를 작성하십시오.

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
  {: codeblock}

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
  {: codeblock}

4. 작성한 카드를 보려면 서비스 대시보드로 이동하십시오.

## 시나리오 예제
{: #example}

사용자 정의를 작성하는 이유는 무엇입니까? 이름이 `cloudkingdom`인 {{site.data.keyword.containershort_notm}} 클러스터가 실행되는 애플리케이션이 있다고 응답하십시오. 클러스터의 팟(Pod) 중 하나는 비정상적인 데이터의 양을 외부 서버에 전송하고 있습니다. {{site.data.keyword.security-advisor_short}} 대시보드에서 이 찾은 결과를 캡처하려고 합니다.

사용자 정의 도구가 전송 중인 비정상적인 데이터의 양을 감지하는 경우 프로토콜은 찾은 결과를 {{site.data.keyword.security-advisor_short}}에 전송합니다.

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
		"service_name": "kubernetess service"
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
{: codeblock}
