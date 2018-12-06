---

copyright:
  years: 2014, 2018
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

# {{site.data.keyword.security-advisor_short}} 설정
{: #setup}

{{site.data.keyword.security-advisor_long}}를 사용하면 보안 위험 및 위협에 대비하여 앱을 계속해서 모니터할 수 있습니다. 취약성이 발견되면 서비스 대시보드를 통해 경보가 발행됩니다.
{:shortdesc}

## 컨테이너 이미지에서 취약성 모니터링
{: #setup_images}

Docker 이미지는 사용자가 작성하는 모든 컨테이너의 기본입니다. 이미지에는 앱, 구성 및 필요한 종속성이 포함될 수 있습니다. 이미지는 일반적으로 레지스트리에 저장됩니다. {{site.data.keyword.registryshort_notm}}를 사용하여 Vulnerability Advisor에 액세스할 수 있으며, 잠재적인 보안 문제에 대비하여 이미지를 계속해서 스캔합니다. 문제가 발견되면 경보가 발행되고 {{site.data.keyword.security-advisor_short}} 대시보드에서 종합 보고서를 볼 수 있습니다.
{:shortdesc}

[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html#index)에 대해 자세히 알아보십시오.


**시작하기 전에**

레지스트리를 시작하기 전에 다음 CLI 및 플러그인이 설치되었는지 확인하십시오. 
- [{{site.data.keyword.Bluemix_notm}} CLI ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://clis.ng.bluemix.net/ui/home.html)
- 컨테이너 레지스트리 플러그인입니다.

    ```
    ibmcloud plugin install container-registry -r Bluemix
    ```
    {: pre}

</br>
**네임스페이스 작성**

1. CLI를 사용하여 계정에 로그인하십시오. 

   ```
   bx login --sso
   ```
   {: pre}

2. {{site.data.keyword.registryshort_notm}}에 로그인하십시오.

   ```
   bx cr login
   ```
   {: pre}

3. 선택사항: 네임스페이스를 작성하십시오. 항상 기존 네임스페이스를 사용할 수 있습니다.

   ```
   bx cr namespace-add
   ```
   {: pre}

3. 이미지에 태그를 지정하십시오. 

   ```
   docker tag <image>:<tag> registry.ng.bluemix.net/<namespace>/<image>:<tag>
   ```
   {: pre}

5. 이미지를 푸시하십시오. 

   ```
   docker push registry.ng.bluemix.net/<namespace>/<image>:<tag>
   ```
   {: pre}


이미지를 {{site.data.keyword.registryshort_notm}} 네임스페이스에 푸시한 후 발견된 취약성에 대한 정보는 서비스 대시보드의 **취약성이 있는 이미지** 카드에 표시됩니다. 또한 특정 이미지를 드릴다운하여 알려진 모든 취약성에 대한 설명 및 식별된 구성 문제에 대한 자세한 정보를 찾을 수도 있습니다. 

</br>

## 인증서 모니터링
{: #setup_certificates}

{{site.data.keyword.cloudcerts_long_notm}}은 SSL/TLS 인증서를 모니터하고 관리하는 데 도움이 된다는 사실을 알고 계십니까? {{site.data.keyword.cloudcerts_short}} 및 {{site.data.keyword.security-advisor_short}}를 통합하여 인증서 및 기타 정보를 업데이트해야 하는 시점에 대해 경보 및 미리 알림을 받을 수 있으며, 이는 나중에 취약성이 발생하지 않도록 도움을 줄 수 있습니다.
{:shortdesc}

[{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager/index.html#gettingstarted)에 대해 자세히 알아볼 수 있습니다.

1. {{site.data.keyword.Bluemix_notm}} 카탈로그에서 "{{site.data.keyword.cloudcerts_short}}"를 검색하십시오.
2. 서비스 인스턴스에 이름을 지정하거나 미리 설정된 이름을 사용하십시오. 
3. **작성**을 클릭하십시오.
4. 조직의 인증서를 {{site.data.keyword.cloudcerts_short}}에 가져오려면 **인증서 가져오기**를 클릭하십시오.

이제 인증서를 가져오게 됨에 따라 만기 시간 및 만료된 인증서와 같은 정보가 {{site.data.keyword.security-advisor_short}} 대시보드의 **인증서** 카드에 표시됩니다. 카드를 클릭하여 인증서가 속하는 서비스 인스턴스와 같은 인증서에 대한 특정한 정보를 가져올 수 있습니다. 또한 보안 취약성을 수정하기 위해 수행할 수 있는 단계를 볼 수도 있습니다. 
