---

copyright:
  years: 2019
lastupdated: "2019-02-18"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# 문제점 해결
{: #troubleshooting}

{{site.data.keyword.security-advisor_long}}에 대한 작업을 수행하는 중에 문제점이 발생하면 문제점 해결하고 도움을 받기 위해 이 기술을 고려하십시오.
{: shortdesc}


## 도움 및 지원 받기
{: #getting-help-and-support}



문제점을 검색하거나 포럼을 통해 질문하여 도움말을 찾을 수 있습니다. 또한 지원 티켓도 열 수 있습니다. 질문하기 위해 포럼을 사용 중인 경우 {{site.data.keyword.Bluemix_notm}} 개발팀에서 볼 수 있도록 질문에 태그를 지정하십시오.
{: shortdesc}

* {{site.data.keyword.security-advisor_short}}에 대한 기술상의 질문이 있으면 질문을 <a href="http://stackoverflow.com/" target="_blank">스택 오버플로우 <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>에 게시하십시오. `security-advisor` 및 `ibm-cloud` 태그를 포함해야 합니다.
* 서비스 및 지시사항 시작에 대한 질문은 <a href="https://developer.ibm.com/" target="_blank">dW 답변 <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a> 포럼을 사용하십시오. `security-advisor` 및 `ibm-cloud` 태그를 포함해야 합니다.

지원 받기에 대한 자세한 정보는 [필요한 지원을 받는 방법](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)을 참조하십시오.


## 사용자 정의 솔루션 발생을 작성할 수 없음
{: #ts-custom-occurrence}

{: tsSymptoms}
사용자 정의 솔루션 발생을 작성하려고 했으나 정보가 브라우저에 표시되지 않고 오류 메시지가 수신되지 않았습니다.

{: tsCauses}
알려진 문제가 발생했습니다. 이미 선택한 이름이 존재하므로 발생 작성에 실패했습니다.

{: tsResolve}
발생에 다른 이름을 선택하십시오.


## 오류: "security-advisor-insights" 네임스페이스 조작이 금지됨
{: #ts-error-helm-install}

{: tsSymptoms}
Network 또는 Activity Insights를 설치하려고 할 때 오류가 발생함:

```
namespaces “security-advisor-insights” is forbidden: User “system:serviceaccount:kube-system:default” cannot get resource “namespaces” in API group “” in the namespace “security-advisor-insights”
```
{: screen}

{: tsCauses}
`kube-system` 기본 서비스 계정에 클러스터의 관리 액세스 권한이 없습니다.

{: tsResolve}
기본 제공 인사이트 오퍼링 중 하나를 설치하기 전에 Helm을 설치해야 합니다. [Kubernetes Service 통합 문서](/docs/containers?topic=containers-integrations#helm)를 사용하여 Helm을 설치할 수 있습니다.


## 알려진 결함: 일부 Network Insights 찾은 결과가 표시되지 않음
{: #ts-network-analytics}

{: tsSymptoms}
대시보드 또는 API를 통해 Network Insights를 찾는 경우 모든 유형의 찾은 결과가 표시되지 않습니다.

{: tsCauses}
일부 Network Insights 찾기 유형은 IBM Cloud Kubernetes Service 버전 1.10 이하에서만 작동합니다.

{: tsResolve}
Kubernetes Service 버전 1.10 이하를 사용하십시오.
