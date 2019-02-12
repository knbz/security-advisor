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

# Network Analytics (Betaversion) konfigurieren
{: #cluster_install}

Sie können die Funktion für die Netzanalyse (Network Analytics) des Service ausprobieren, indem Sie einen {{site.data.keyword.security-advisor_short}} in einem {{site.data.keyword.containerlong_notm}}-Cluster installieren.
{: shortdesc}

Network Analytics ist eine Vorschaufunktion, die nur in einer Region im Süden der USA verfügbar ist (Region 'US-South').
{: note}

Wenn Sie {{site.data.keyword.security-advisor_short}} in einem Cluster konfigurieren, werden folgende Aktionen ausgeführt:

* Clustermetadaten werden erfasst und für die Installation der folgenden Elemente verwendet: IP-Adressen des Workerknotens, Teilnetze, Ingress-URL und -IP-Adresse, Cluster-CRN, Log Analysis-Endpunkt, Bereich und Token.
* In Ihrem {{site.data.keyword.Bluemix_notm}}-Konto werden eine IAM-Service-ID und ein IAM-API-Schlüssel (IAM – Identity and Access Management) generiert, sodass Ihr Cluster von {{site.data.keyword.security-advisor_short}} identifiziert werden kann.
* Die {{site.data.keyword.security-advisor_short}}-Komponenten werden im Namensbereich **monitoring** in Ihrem Cluster bereitgestellt.

Wenn Sie über mehrere Cluster verfügen, müssen Sie sicherstellen, dass die Installation für jeden einzelnen der Cluster ausgeführt wird, damit für jeden Cluster Service-IDs und API-Schlüssel generiert werden.


Weitere Informationen zur Funktionsweise der Arbeit von Network Analytics in {{site.data.keyword.security-advisor_short}} erhalten.[Dann schauen Sie sich die Dokumentation an](network-analytics.html).
{: tip}

</br>

## Voraussetzungen
{: #cluster_prereqs}

Stellen Sie sicher, dass vor dem Start folgende Voraussetzungen erfüllt sind.
{: shortdesc}

Der Kontoeigner muss die Installationsschritte ausführen. Wenn nicht Sie der Eigner des {{site.data.keyword.Bluemix_notm}}-Kontos sind, müssen Sie diesen ermitteln und sich zur Unterstützung der Installation an ihn wenden.
{: tip}

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Mac-, Linux- oder [Windows 10](https://win10faq.com/install-run-ubuntu-bash-windows-10/)-Entwicklerworkstation.
* [Node.js](https://nodejs.org/en/) 6 oder höher
* [jQ](https://stedolan.github.io/jq/download/)
* Die erforderlichen CLIs und Plug-ins in der mindestens erforderlichen Version:
  * [{{site.data.keyword.Bluemix_notm}}-CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started) Version 0.6.5 oder höher
  * [Kubernetes-CLI (kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) Version 1.7 oder höher</br> Das Installationsprogramm wurde mit der standardmäßigen und stabilen Version `v1.10.11` von {{site.data.keyword.containerlong_notm}} getestet. Das Installationsprogramm wurde nicht mit `v1.11.5` oder der neuesten Version `v1.12.3` getestet.
  * [{{site.data.keyword.containerlong_notm}}-Plug-in](https://console.bluemix.net/docs/containers/cs_cli_install.html#cs_cli_install)
  * [Kubernetes Helm (Paketmanager)](https://docs.helm.sh/using_helm/#from-script)
* [Ein kostenloser oder standardmäßiger Kubernetes-Cluster](https://console.bluemix.net/containers-kubernetes/catalog/cluster) in der {{site.data.keyword.Bluemix_notm}}-Region **US-South**. 

Benötigen Sie Hilfe bei der Erstellung oder Konfiguration eines Clusters? Versuchen Sie, dieses [Lernprogramm](/docs/containers/cs_tutorials.html#cs_cluster_tutorial) durchzuarbeiten.
{: tip}

</br>

## Helm einrichten
{: #login}

1.  Melden Sie sich bei {{site.data.keyword.Bluemix_notm}} an.

  ```
  ibmcloud login -a https://api.ng.bluemix.net --sso
  ```
  {: pre}

2.  Listen Sie alle Cluster im Konto auf, um den Namen des Clusters abzurufen, mit dem Sie arbeiten möchten.

  ```
  ibmcloud ks clusters
  ```
  {: pre}

3.  Geben Sie den Cluster in der Befehlszeilenschnittstelle als Ziel an.

  1. Führen Sie zum Konfigurieren des Clusters folgenden Befehl aus. Die Ausgabe benötigen Sie im nächsten Schritt.

    ```
    ibmcloud ks cluster-config <Clustername-oder--ID>
    ```
    {: pre}

  2. Verwenden Sie die Ausgabe des vorherigen Befehls, um den Pfad zur lokalen Kubernetes-Konfigurationsdatei als Umgebungsvariable festzulegen.

  Beispiel:

    ```
    export KUBECONFIG=/Users/<Benutzername>/.bluemix/plugins/container-service/clusters/<Clustername>/kube-config-prod-dal10-<Clustername>.yml
    ```
    {: screen}

4.  Installieren Sie Tiller in Ihrem Cluster, damit Sie mit Helm-Diagrammen arbeiten können.

  ```
  helm init
  ```
  {: pre}

Stellen Sie sicher, dass dieses Befehlszeilenfenster geöffnet bleibt, wenn Sie fortfahren.
{: tip}

</br>

## {{site.data.keyword.security-advisor_short}}-Komponenten installieren
{: #cluster_components}

Wenn Ihr Cluster für die Arbeit mit Helm konfiguriert ist, können Sie {{site.data.keyword.security-advisor_short}}-Komponenten installieren.
{: shortdesc}


Zum Installieren der Komponenten fahren Sie in demselben Befehlszeilenschnittstellenfenster fort und führen Sie folgende Schritte aus:

1. Laden Sie das [Installationspaket](https://github.com/IBM-Bluemix-Docs/security-advisor/blob/master/installation.tar.gz?raw=true) herunter und extrahieren Sie es.
2. Fahren Sie in demselben Befehlszeilenschnittstellenfenster wie im vorherigen Abschnitt fort, navigieren Sie zu der Position des extrahierten Archivs und installieren Sie das Paket.

  ```
  ./install.sh
  ```
  {: pre}

3.  Überprüfen Sie die ordnungsgemäße Installation der Komponenten im [{{site.data.keyword.security-advisor_short}}-Dashboard](https://console.bluemix.net/security-advisor/#/dashboard).

</br>

## {{site.data.keyword.security-advisor_short}}-Komponenten entfernen
{: #cluster_uninstall}

Entfernen Sie die {{site.data.keyword.security-advisor_short}}-Komponenten aus Ihrem Cluster sowie die Service-ID und den API-Schlüssel aus Ihrem {{site.data.keyword.Bluemix_notm}}-Konto.

1. Melden Sie sich bei {{site.data.keyword.Bluemix_notm}} an.

  ```
  ibmcloud login -a https://api.ng.bluemix.net --sso
  ```
  {: pre}

2. Listen Sie alle Cluster im Konto auf, um den Namen des Clusters abzurufen.

  ```
  ibmcloud ks clusters
  ```
  {: pre}

3. Geben Sie den Cluster in der Befehlszeilenschnittstelle als Ziel an.

  ```
  ibmcloud ks cluster-config <Clustername-oder--ID>
  ```
  {: pre}

4. Definieren Sie den Pfad zur lokalen Kubernetes-Konfigurationsdatei als Umgebungsvariable. Beispiel:

  ```
  export KUBECONFIG=/Users/<Benutzername>/.bluemix/plugins/container-service/clusters/<Clustername>/kube-config-prod-dal10-<Clustername>.yml
  ```
  {: pre}

5. Navigieren Sie zu der Position des extrahierten Archivs und führen Sie das Deinstallationsscript aus.

  ```
  ./uninstall.sh
  ```
  {: pre}

6. Deinstallieren Sie wahlweise die Helm-Serverkomponente im Cluster.

  ```
  helm reset
  ```
  {: pre}
