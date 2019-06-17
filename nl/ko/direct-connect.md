---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

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


# 직접 연결
{: #setup-custom-gui}

{{site.data.keyword.security-advisor_short}}를 사용하면 오픈 소스, 사용자 정의 개발 또는 서드파티 서비스 등 기존 사용자 정의 보안 도구를 통합할 수 있습니다. 통합 목록에 URL을 추가하여 {{site.data.keyword.security-advisor_short}}에서 다른 도구로 직접 연결을 생성할 수 있습니다. 링크를 작성하면 사용하는 모든 도구를 쉽게 모니터할 수 있습니다.
{: shortdesc}


## 시작하기 전에
{: #custom-before-gui}

통합을 추가하기 전에 통합하려는 파트너에 대한 계정이 있어야 합니다.

{{site.data.keyword.security-advisor_short}}는 파트너 서비스와 관련된 어떤 인증 정보도 지속시키지 않습니다. 엔터프라이즈 사용자는 SAML을 사용하여 {{site.data.keyword.cloud_notm}} 및 비즈니스 파트너에 모두 인증해야 합니다.
{: note}

## 연결 구성
{: #custom-configure-connection}

1. 보안 도구에 로그인한 후 고유 URL을 가져오십시오.

2. 콘솔을 사용하여 {{site.data.keyword.cloud_notm}}에 로그인하십시오.

3. **사용자 정의 통합 > 직접 연결**을 클릭하십시오. 화면이 표시됩니다.

  1. 솔루션 이름을 제공하십시오. 이름에 영숫자 문자, 공백 및 대시(-)만 사용할 수 있습니다.

  2. `www.<website>.<domain>` 형식으로 솔루션의 URL을 입력하십시오.

  3. 도구를 나타내는 아이콘 또는 이미지를 업로드하십시오.

  4. **연결**을 클릭하여 구성을 완료하십시오. {{site.data.keyword.security-advisor_short}}는 통합에 필요한 아티팩트(예: 서비스 ID, API 키, 계정 ID 및 메타데이터)를 작성합니다. `작성자` 역할이 지정됩니다.
