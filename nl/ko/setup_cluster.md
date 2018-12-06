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

# Kubernetes 클러스터에 대한 의심스러운 클라이언트 및 서버 IP의 모니터링 설정
{: #cluster_install}

네트워크 분석 기능을 사용하려면 {{site.data.keyword.containerlong_notm}}에 배치된 클러스터에 {{site.data.keyword.security-advisor_short}} 컴포넌트를 설치하십시오. 그런 다음 {{site.data.keyword.security-advisor_short}} 대시보드에서 네트워크 인사이트 및 경보를 볼 수 있습니다.
{:shortdesc}

이 {{site.data.keyword.security-advisor_short}} 기능은 기술 미리보기입니다.
{: tip}

{{site.data.keyword.security-advisor_short}}의 네트워크 분석에 대해 좀 더 자세히 알고 싶으십니까? [문서를 확인하십시오](network-analytics.html).


## 선행 조건
{: #cluster_prereqs}

시작하기 전에:

* Mac, Linux 또는 Windows 10 개발자 워크스테이션
  * Windows 10: [Linux 서브시스템 기능 사용](https://win10faq.com/install-run-ubuntu-bash-windows-10/)
* [Node.js](https://nodejs.org/en/) 6 이상
* [jQ](https://stedolan.github.io/jq/download/)
* [{{site.data.keyword.Bluemix_notm}} 명령행 인터페이스](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started) v0.6.5 이상
* [{{site.data.keyword.containerlong_notm}} CLI 플러그인](https://console.bluemix.net/docs/containers/cs_cli_install.html#cs_cli_install)
* [Kubernetes CLI(kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.7 이상
* [Kubernetes Helm(패키지 관리자)](https://docs.helm.sh/using_helm/#from-script)
* {{site.data.keyword.Bluemix_notm}}의 **미국 남부** 지역에 있는 [비어 있거나 표준 Kubernetes 클러스터](https://console.bluemix.net/containers-kubernetes/catalog/cluster)
* {{site.data.keyword.Bluemix_notm}} 계정 소유자를 식별하여 설치 단계 완료

## 클러스터에 로그인
{: #login}

1.  {{site.data.keyword.Bluemix_notm}}에 로그인하십시오.

    ```
    ibmcloud login -a https://api.ng.bluemix.net --sso
    ```
    {: pre}

2.  클러스터의 이름을 가져오려면 계정에 있는 모든 클러스터를 나열하십시오. 

    ```
    ibmcloud ks clusters
    ```
    {: pre}

3.  클러스터에 대한 CLI를 대상으로 지정하십시오.

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

4.  환경 변수로서 경로를 로컬 Kubernetes 구성 파일로 설정하십시오. 예를 들면, 다음과 같습니다.

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: pre}

5.  클러스터에서 Helm을 설정하십시오. 

    ```
    helm init
    ```
    {: pre}

이 명령행 창을 열어 두고 계속 수행하십시오.
{: tip}

## Security Advisor 컴포넌트 설치
{: #cluster_components}

{{site.data.keyword.Bluemix_notm}} 계정 소유자가 설치 프로그램을 실행해야 합니다.
{: tip}

{{site.data.keyword.security-advisor_short}}를 설정할 때 다음 조치가 수행됩니다. 
1. 작업자 노드 IP 주소, 서브넷, Ingress URL과 IP 주소, 클러스터 CRN, 로그 분석 엔드포인트, 영역 및 토큰의 설치를 완료하기 위해 클러스터 메타데이터가 수집되고 사용됩니다. 
2. 클러스터가 Security Advisor로 식별될 수 있도록 IAM 서비스 ID 및 API 키가 {{site.data.keyword.Bluemix_notm}} 계정에 생성됩니다. 둘 이상의 클러스터가 있는 경우 각 클러스터마다 설치를 실행하여 각 클러스터에 대한 서비스 ID 및 API 키를 생성하십시오.
3. {{site.data.keyword.security-advisor_short}} 컴포넌트는 클러스터의 **모니터링** 네임스페이스에 배치됩니다. 

<br/>
{{site.data.keyword.security-advisor_short}} 컴포넌트를 설치하려면 다음을 수행하십시오.

1.  [설치 패키지](https://github.com/IBM-Bluemix-Docs/security-advisor/blob/master/installation.tar.gz?raw=true)를 다운로드하고 추출하십시오.
2.  이전 섹션과 동일한 명령행 창에서 추출된 아카이브 위치로 이동하여 패키지를 설치하십시오.

    ```
    ./install.sh
    ```
    {: pre}

3.  [{{site.data.keyword.security-advisor_short}} 대시보드](https://console.bluemix.net/security-advisor/#/dashboard)를 확인하여 컴포넌트가 올바르게 설치되었는지 확인하십시오.

## {{site.data.keyword.security-advisor_short}} 컴포넌트 제거
{: #cluster_uninstall}

{{site.data.keyword.Bluemix_notm}} 계정의 클러스터 및 서비스 ID와 API 키에서 {{site.data.keyword.security-advisor_short}} 컴포넌트를 제거하십시오.

1.  {{site.data.keyword.Bluemix_notm}}에 로그인하십시오.

    ```
    ibmcloud login -a https://api.ng.bluemix.net --sso
    ```
    {: pre}

2.  클러스터의 이름을 가져오려면 계정에 있는 모든 클러스터를 나열하십시오. 

    ```
    ibmcloud ks clusters
    ```
    {: pre}

3.  클러스터에 대한 CLI를 대상으로 지정하십시오.

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

4.  환경 변수로서 경로를 로컬 Kubernetes 구성 파일로 설정하십시오. 예를 들면, 다음과 같습니다.

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: pre}

5.  추출된 아카이브 위치로 이동하고 설치 제거 프로그램 스크립트를 실행하십시오. 

    ```
    ./uninstall.sh
    ```
    {: pre}

6.  선택사항: 클러스터에서 Helm 서버 컴포넌트를 설치 제거하십시오.

    ```
    helm reset
    ```
    {: pre}
