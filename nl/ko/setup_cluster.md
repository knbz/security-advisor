---

copyright:
  years: 2018
lastupdated: "2018-12-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:note: .note}

# 네트워크 분석(베타) 설정
{: #cluster_install}

{{site.data.keyword.containerlong_notm}} 클러스터에 {{site.data.keyword.security-advisor_short}}를 설치하여 이 서비스의 네트워크 분석 기능을 사용해 볼 수 있습니다.
{: shortdesc}

네트워크 분석은 미국 남부 지역에서만 사용할 수 있는 미리보기 기능입니다.
{: note}

클러스터에서 {{site.data.keyword.security-advisor_short}}를 설정하면 다음과 같은 조치가 수행됩니다.

* 작업자 노드 IP 주소, 서브넷, Ingress URL과 IP 주소, 클러스터 CRN, 로그 분석 엔드포인트, 영역 및 토큰의 설치를 완료하기 위해 클러스터 메타데이터가 수집되고 사용됩니다.
* {{site.data.keyword.security-advisor_short}}에서 클러스터를 식별할 수 있도록 {{site.data.keyword.Bluemix_notm}} 계정에 IAM(Identity and Access Management) 서비스 ID 및 API 키가 생성됩니다.
* 클러스터의 **모니터링** 네임스페이스에 {{site.data.keyword.security-advisor_short}} 컴포넌트가 배치됩니다.

둘 이상의 클러스터가 존재하는 경우 각각의 클러스터에 대한 설치를 실행하여 각각의 클러스터에 대한 서비스 ID 및 API 키를 생성해야 합니다.


{{site.data.keyword.security-advisor_short}}에서 네트워크 분석이 작동하는 방식에 대한 자세한 정보를 확인하시겠습니까? [문서를 확인하십시오](network-analytics.html).
{: tip}

</br>

## 선행 조건
{: #cluster_prereqs}

시작하기 전에 다음과 같은 전제조건이 준비되어 있는지 확인하십시오.
{: shortdesc}

계정 소유자는 설치 단계를 완료해야 합니다. 계정 소유자가 아닌 경우 {{site.data.keyword.Bluemix_notm}} 계정의 소유자가 누구인지 확인한 후 설치를 지원하도록 요청하십시오.
{: tip}

그런 다음 다음과 같은 전제조건이 준비되어 있는지 확인하십시오.

* Mac, Linux 또는 [Windows 10](https://win10faq.com/install-run-ubuntu-bash-windows-10/) 개발자 워크스테이션
* [Node.js](https://nodejs.org/en/) 6 이상
* [jQ](https://stedolan.github.io/jq/download/)
* 최소 필수 버전의 필수 CLI 및 플러그인:
  * [{{site.data.keyword.Bluemix_notm}} CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started) v0.6.5 이상
  * [Kubernetes CLI(kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.7 이상</br> 설치 프로그램은 {{site.data.keyword.containerlong_notm}} 기본 안정화 버전 `v1.10.11`에서 테스트되었습니다. `v1.11.5` 또는 최신 버전인 `v1.12.3`에서는 설치 프로그램이 테스트되지 않았습니다.
  * [{{site.data.keyword.containerlong_notm}} 플러그인](https://console.bluemix.net/docs/containers/cs_cli_install.html#cs_cli_install)
  * [Kubernetes Helm(패키지 관리자)](https://docs.helm.sh/using_helm/#from-script)
* **미국 남부** {{site.data.keyword.Bluemix_notm}}의 경우 [무료 또는 표준 Kubernetes 클러스터](https://console.bluemix.net/containers-kubernetes/catalog/cluster)

클러스터를 작성 및 구성하는 데 도움이 필요하십니까? 이 [튜토리얼](/docs/containers/cs_tutorials.html#cs_cluster_tutorial)을 실행해 보십시오.
{: tip}

</br>

## Helm 설정
{: #login}

1.  {{site.data.keyword.Bluemix_notm}}에 로그인하십시오.

  ```
    ibmcloud login -a https://api.ng.bluemix.net --sso
  ```
  {: pre}

2.  계정에 있는 모든 클러스터를 나열하여 작업을 수행할 클러스터의 이름을 가져오십시오.

  ```
    ibmcloud ks clusters
  ```
  {: pre}

3.  클러스터에 대한 CLI를 대상으로 지정하십시오.

  1. 다음 명령을 실행하여 클러스터를 구성하십시오. 다음 단계에서 이 출력을 사용하게 됩니다.

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

  2. 이전 명령의 출력을 사용하여 로컬 Kubernetes 구성 파일의 경로를 환경 변수로 설정하십시오.

  예를 들면, 다음과 같습니다.

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: screen}

4.  Helm 차트 관련 작업을 수행할 수 있도록 클러스터에 틸러를 설치하십시오.

  ```
    helm init
  ```
  {: pre}

이 명령행 창을 계속 열어 두어야 합니다.
{: tip}

</br>

## {{site.data.keyword.security-advisor_short}} 컴포넌트 설치
{: #cluster_components}

Helm에서 작동하도록 클러스터가 구성된 후에는 {{site.data.keyword.security-advisor_short}} 컴포넌트를 설치할 수 있습니다.
{: shortdesc}


컴포넌트를 설치하려면 동일한 CLI 창에서 계속 진행하여 다음 단계를 완료하십시오.

1. [설치 패키지](https://github.com/IBM-Bluemix-Docs/security-advisor/blob/master/installation.tar.gz?raw=true)를 다운로드하고 추출하십시오.
2. 이전 절과 동일한 CLI 창에서 계속 진행하여 추출된 아카이브 위치로 이동한 후 패키지를 설치하십시오.

  ```
    ./install.sh
  ```
  {: pre}

3.  [{{site.data.keyword.security-advisor_short}} 대시보드](https://console.bluemix.net/security-advisor/#/dashboard)를 검사하여 컴포넌트가 올바르게 설치되었는지 확인하십시오.

</br>

## {{site.data.keyword.security-advisor_short}} 컴포넌트 제거
{: #cluster_uninstall}

{{site.data.keyword.Bluemix_notm}} 계정의 클러스터 및 서비스 ID와 API 키에서 {{site.data.keyword.security-advisor_short}} 컴포넌트를 제거하십시오.

1. {{site.data.keyword.Bluemix_notm}}에 로그인하십시오.

  ```
    ibmcloud login -a https://api.ng.bluemix.net --sso
  ```
  {: pre}

2. 클러스터의 이름을 가져오려면 계정에 있는 모든 클러스터를 나열하십시오.

  ```
    ibmcloud ks clusters
  ```
  {: pre}

3. 클러스터에 대한 CLI를 대상으로 지정하십시오.

  ```
    ibmcloud ks cluster-config <cluster-name-or-id>
  ```
  {: pre}

4. 환경 변수로서 경로를 로컬 Kubernetes 구성 파일로 설정하십시오. 예를 들면, 다음과 같습니다.

  ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
  ```
  {: pre}

5. 추출된 아카이브 위치로 이동하고 설치 제거 프로그램 스크립트를 실행하십시오.

  ```
    ./uninstall.sh
  ```
  {: pre}

6. 선택사항: 클러스터에서 Helm 서버 컴포넌트를 설치 제거하십시오.

  ```
    helm reset
  ```
  {: pre}
