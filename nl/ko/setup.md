---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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

# 사전 통합 서비스 활용
{: #setup-services}

{{site.data.keyword.security-advisor_short}}는 보안 위험 및 위협을 모니터할 수 있도록 해주는 몇 가지 미리 채워진 카드와 함께 제공됩니다.
{: shortdesc}

{{site.data.keyword.security-advisor_short}}는 다음과 같은 서비스에 대해 자동으로 카드를 작성합니다.

* {{site.data.keyword.registrylong_notm}}
* {{site.data.keyword.cloudcerts_long_notm}}

{{site.data.keyword.security-advisor_short}}와 서비스 간의 연결을 작성하기 위해 별도의 작업을 수행할 필요는 없지만 서비스의 인스턴스가 해당 정보로 구성되도록 해야 합니다.


## 컨테이너 이미지에서 취약성 모니터링
{: #setup-images}

{{site.data.keyword.registryshort_notm}}를 통해 Vulnerability Advisor에 액세스하여 {{site.data.keyword.registryshort_notm}} 인스턴스에서 잠재적 보안 문제를 지속적으로 스캔할 수 있습니다. 문제가 발견되면 경보가 발행되고 {{site.data.keyword.security-advisor_short}} 대시보드에서 종합 보고서를 볼 수 있습니다.
{:shortdesc}

[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started)에 대해 자세히 알아보십시오.


### 시작하기 전에
{: #setup-before}

레지스트리를 시작하기 전에 다음 CLI 및 플러그인이 설치되었는지 확인하십시오.
* [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli)
* 컨테이너 레지스트리 플러그인입니다.

  ```
  ibmcloud plugin install container-registry
  ```
  {: codeblock}


### 네임스페이스 작성
{: #setup-create-namespace}

1. CLI를 사용하여 계정에 로그인하십시오.

  ```
  ibmcloud login --sso
  ```
  {: codeblock}

2. {{site.data.keyword.registryshort_notm}}에 로그인하십시오.

  ```
  ibmcloud cr login
  ```
  {: codeblock}

3. 선택사항: 네임스페이스를 작성하십시오. 항상 기존 네임스페이스를 사용할 수 있습니다.

  ```
  ibmcloud cr namespace-add
  ```
  {: codeblock}

3. 이미지에 태그를 지정하십시오.

  ```
  docker tag <image>:<tag> <region>.icr.io/<namespace>/<image>:<tag>
  ```
  {: codeblock}

5. 이미지를 푸시하십시오.

  ```
  docker push <region>.icr.io/<namespace>/<image>:<tag>
  ```
  {: codeblock}


이미지를 {{site.data.keyword.registryshort_notm}} 네임스페이스에 푸시한 후 발견된 취약성에 대한 정보는 서비스 대시보드의 **취약성이 있는 이미지** 카드에 표시됩니다. 또한 특정 이미지로 드릴다운하여 모든 식별된 취약성 및 구성 문제에 대한 설명 등의 자세한 정보를 찾을 수 있습니다.


## 인증서 모니터링
{: #setup-certificates}

{{site.data.keyword.cloudcerts_long_notm}}은 SSL/TLS 인증서를 모니터하고 관리하는 데 도움이 된다는 사실을 알고 계십니까? {{site.data.keyword.cloudcerts_short}} 및 {{site.data.keyword.security-advisor_short}}를 통합하는 경우 서비스 또는 애플리케이션 가동 중단을 방지할 수 있도록 인증서 만기에 대한 경보를 사전에 수신할 수 있습니다.
{:shortdesc}

{{site.data.keyword.cloudcerts_short}}에 업로드한 인증서의 만기 데이터에 따라 인증서가 만료되기 90, 60, 10 및 1일 전에 {{site.data.keyword.security-advisor_short}} 대시보드에 찾은 결과가 표시됩니다. 또한 만료된 인증서에 대한 일별 알림을 수신합니다. 일별 알림은 인증서가 만료된 다음 날 시작됩니다.

수동 업데이트를 트리거하려는 경우 {{site.data.keyword.cloudcerts_short}} 인스턴스에 하루 뒤에 만료되는 인증서를 업로드할 수 있습니다. 가져오기가 정상적으로 완료되면 {{site.data.keyword.security-advisor_short}} 대시보드에 주요 위험 지표(KRI) 및 찾은 결과가 표시됩니다.

문서를 읽어서 [{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager?topic=certificate-manager-getting-started)에 대한 자세한 정보를 확인할 수 있습니다.
{: tip}

### 인증서 작성
{: #setup-create-cert}

하루 뒤에 만료되는 자체 서명 인증서를 작성하려는 경우 터미널에서 다음 OpenSSL 명령을 실행할 수 있습니다.

```
openssl req -x509 -newkey rsa:2048 -keyout key.pem -subj "/CN=myservice.com" -out server.pem -days 1 -nodes
```
{: codeblock}


### 인증서 업로드
{: #setup-upload-cert}

1. {{site.data.keyword.cloud_notm}} 카탈로그에서 "{{site.data.keyword.cloudcerts_short}}"를 검색하십시오.
2. 서비스 인스턴스에 이름을 지정하거나 미리 설정된 이름을 사용하십시오.
3. **작성**을 클릭하십시오.
4. 조직의 인증서를 {{site.data.keyword.cloudcerts_short}}에 가져오려면 **인증서 가져오기**를 클릭하십시오.

이제 인증서를 가져오게 됨에 따라 만기 시간 및 만료된 인증서와 같은 정보가 {{site.data.keyword.security-advisor_short}} 대시보드의 **인증서** 카드에 표시됩니다. 카드를 클릭하여 인증서가 속하는 서비스 인스턴스와 같은 인증서에 대한 특정한 정보를 가져올 수 있습니다. 또한 보안 취약성을 수정하기 위해 수행할 수 있는 단계를 볼 수도 있습니다.

알림을 중지하려면 인증서를 갱신하고 {{site.data.keyword.cloudcerts_short}}에 해당 인증서를 업로드한 후 만료된 인증서를 삭제해야 합니다.
{: tip}
