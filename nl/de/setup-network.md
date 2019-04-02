---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-13"

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

# Network Insights
{: #setup-network}

Mit {{site.data.keyword.security-advisor_long}} können Sie unter Verwendung von maschinellem Lernen, erlernten Mustern und Informationen zu Sicherheitsbedrohungen das Verhalten überwachen, um potenziell beeinträchtigte Container zu ermitteln, deren Ausführung in Ihren {{site.data.keyword.containerlong_notm}}-Clustern erfolgt.
{: shortdesc}

Ab dem 20. Januar 2019 ersetzt Network Insights (Betaversion) das Network Analytics-Feature. Es werden alle Analysekarten in Ihrem Service-Dashboard gelöscht, doch die Untersuchungsergebnisse bleiben weiterhin in der Datenbank für Untersuchungsergebnisse erhalten.
{: important}

## Vorbereitende Schritte
{: #network-prereq}

Wenn Sie gegenwärtig das Network Analytics-Feature verwenden, müssen Sie die [Servicekomponenten löschen](/docs/services/security-advisor?topic=security-advisor-setup-network#uninstall-analytics), bevor Network Insights installiert werden kann. Um Network Insights verwenden zu können, müssen Sie sicherstellen, dass die folgenden Voraussetzungen erfüllt sind.

- Wenn Sie unter Windows 10 arbeiten, ist es erforderlich, das Windows-Subsystem für Linux zu aktivieren und eine [Ubuntu-Shell](https://win10faq.com/install-run-ubuntu-bash-windows-10/) zu installieren.
- yq-CLI:
  * Installieren Sie die yq-CLI für [macOS und Windows 10](http://mikefarah.github.io/yq/).
  * Führen Sie für CentOS, Red Hat und Ubuntu die folgenden Befehle aus, um Version 1.15 zu installieren:
    ```
    wget https://github.com/mikefarah/yq/releases/download/1.15.0/yq_linux_amd64       
    mv yq_linux_amd64 yq   
    chmod +x yq    
    mv yq /usr/local/bin/     
    yq -V
    ```
    {: codeblock}     
- Aktualisierte cURL-Binärdatei: Für CentOS und Red Hat können Sie die Aktualisierung durch Ausführen von `yum update -y nss curl libcurl` vornehmen.
- [{{site.data.keyword.cloud_notm}}-CLI und erforderliche Plug-ins](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)
- [Kubernetes-CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/) der Version 1.10.11 oder höher
- [Kubernetes Helm (Paketmanager)](/docs/containers?topic=containers-integrations#helm) der Version 2.9.0 oder höher
- Standardmäßiger Kubernetes-Cluster der Version 1.10.11 oder höher

## COS-Bucket erstellen
{: #network-setup-cos}

Durch Verwendung der {{site.data.keyword.security-advisor_short}}-GUI können Sie eine neue COS-Instanz und ein entsprechendes Bucket erstellen.

1. Schalten Sie im Service-Dashboard auf der Registerkarte **Integrationen** die Option **Analyse inaktiviert** im Feld 'Network Insights' zu **Analyse aktiviert** um.

2. Klicken Sie auf **Zur Konfiguration wechseln**.

3. Klicken Sie im Abschnitt für Voraussetzungen auf **COS-Instanz und -Bucket erstellen**. Ihre COS-Instanz und das zugehörige Bucket werden unter Beachtung der entsprechenden Namenskonvention und mit IAM-Berechtigungen automatisch erstellt.

Wenn Sie über eine bereits vorhandene Instanz von COS mit zugehörigem Bucket verfügen, stellen Sie sicher, dass diese die folgende Namenskonvention verwendet: `sa.<konto-ID>.telemetric.<COS-region>`. Damit der Service die in Ihrer COS-Instanz gespeicherten Daten lesen kann, ist es erforderlich, mit {{site.data.keyword.cloud_notm}} IAM die [Service-zu-Service-Autorisierung](/docs/iam?topic=iam-serviceauth#serviceauth) einzurichten. Legen Sie für `source` `{{site.data.keyword.security-advisor_short}}` als Quelle und für `target` Ihre COS-Instanz als Ziel fest. Weisen Sie die IAM-Rolle `Leseberechtigter` (Reader) zu.

## {{site.data.keyword.security-advisor_short}}-Komponenten installieren
{: #network-install-components}

Sie können einen Agenten installieren, um Netzflussprotokolle aus Ihrem Kubernetes-Cluster zu erfassen. Die Protokolle werden in Ihrer Cloud Object Storage-Instanz gespeichert. Dann können Sie Network Insights aktivieren, damit Ihre Protokolle analysiert sowie verdächtige Netzwerkaktivitäten erkannt und an Sie gemeldet werden.
{: shortdesc}

Stellen Sie sicher, dass Sie die Installation für jeden Cluster wiederholen, der überwacht werden soll.
{: note}

1. Klonen Sie das folgende Repository auf Ihrem lokalen System.

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-network-insights.git
  ```
  {: codeblock}

2. Wechseln Sie in den Ordner `security-advisor-network-analytics`.

3. Extrahieren Sie die`.tar`-Datei durch Ausführen des folgenden Befehls:

  ```
  tar -xvf security-advisor-network-insights.tar
  ```
  {: codeblock}

4. Wechseln Sie in den Ordner `security-advisor-network-insights`.

5. Melden Sie sich bei der {{site.data.keyword.cloud_notm}}-CLI an. Gehen Sie gemäß den Eingabeaufforderungen in der CLI vor, um die Anmeldung abzuschließen.

  ```
  ibmcloud login -a https://api.<region>.bluemix.net
  ```
  {: codeblock}

  <table>
    <tr>
      <th>Region</th>
      <th>Endpunkt</th>
    </tr>
    <tr>
      <td>Vereintes Königreich</td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>Vereinigte Staaten (Süden)</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

6. Legen Sie den Kontext für Ihren Cluster fest.

  1. Rufen Sie den Befehl zum Festlegen der Umgebungsvariablen ab und laden Sie die Konfigurationsdateien für Kubernetes herunter.

    ```
    ibmcloud ks cluster-config <clustername_oder_-ID>
    ```
    {: codeblock}

  2. Kopieren Sie die Ausgabe ab einschließlich `export` und fügen Sie sie in Ihrem Terminal ein, um die Umgebungsvariable `KUBECONFIG` festzulegen.

7. Installieren Sie Helm unter Berücksichtigung der [Dokumente zur Integration des Kubernetes-Service](/docs/containers?topic=containers-integrations#helm).

8. Optional: [Aktivieren Sie TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md). Wenn Sie Ihre Workstation für die Abwicklung der Installation von Analysekomponenten in mehreren Clustern verwenden und TLS aktiviert ist, müssen Sie sicherstellen, dass die TLS-Konfigurationen auf dem neuesten Stand sind und mit dem aktuellen Cluster übereinstimmen, in dem Sie die Komponenten installieren möchten.

9. Führen Sie den folgenden Befehl aus, um das Helm-Diagramm und seine Abhängigkeiten zu installieren. Der Befehl überprüft, ob Ihr Bucket die richtige Namenskonvention verwendet, erstellt geheime Schlüssel für Kubernetes, aktualisiert die Werte mit Ihrer Cluster-GUID und stellt das Helm-Diagramm von Network Insights bereit. Falls Sie einen Fehler feststellen, versuchen Sie, den Befehl `helm init --upgrade` auszuführen.

  ```
  ./network-insight-install.sh <COS-region> <COS-API-schlüssel>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>Variable</th>
      <th>Beschreibung</th>
    </tr>
    <tr>
      <td><code>COS-region</code></td>
      <td>Die Region, in der Ihre COS-Instanz bereitgestellt wird. Folgende Optionen sind möglich: <code>us-south</code> und <code>eu-gb</code>.</td>
    </tr>
    <tr>
      <td><code>COS-API-schlüssel</code></td>
      <td>Der [API-Schlüssel](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials#service-credentials), den Sie zum Zugreifen auf Ihre COS-Instanz und das Bucket erstellt haben. Dem Schlüssel muss die Plattformrolle des Autors (`Writer`) zugeordnet sein.</td>
    </tr>
  </table>

Sie haben Ihren Cluster erfolgreich für die Zusammenarbeit mit {{site.data.keyword.security-advisor_short}} Network Insights konfiguriert!



## Komponenten löschen
{: #network-delete}

Wenn Sie Network Insights nicht mehr benötigen, können Sie die Servicekomponenten aus Ihrem Cluster löschen.
{: shortdesc}

1. Melden Sie sich bei der {{site.data.keyword.cloud_notm}}-CLI an. Gehen Sie gemäß den Eingabeaufforderungen in der CLI vor, um die Anmeldung abzuschließen.

  ```
  ibmcloud login -a https://api.<region>.bluemix.net
  ```
  {: codeblock}

  <table>
    <tr>
      <th>Region</th>
      <th>Endpunkt</th>
    </tr>
    <tr>
      <td>Vereintes Königreich</td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>Vereinigte Staaten (Süden)</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

2. Legen Sie den Kontext für Ihren Cluster fest.

  1. Rufen Sie den Befehl zum Festlegen der Umgebungsvariablen ab und laden Sie die Konfigurationsdateien für Kubernetes herunter.

    ```
    ibmcloud ks cluster-config <clustername_oder_-ID>
    ```
    {: codeblock}

  2. Kopieren Sie die Ausgabe ab einschließlich `export` und fügen Sie sie in Ihrem Terminal ein, um die Umgebungsvariable `KUBECONFIG` festzulegen.

3. Löschen Sie die Komponenten mit Helm.

  ```
  helm del --purge network-insights [--tls]
  ```
  {: codeblock}

4. Löschen Sie die geheimen Schlüssel für Kubernetes.

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}

Stellen Sie sicher, dass Sie den Prozess für jeden Cluster löschen, von dem die Agenten entfernt werden sollen.
{: tip}

## Network Analytics deinstallieren
{: #uninstall-analytics}

Wenn Sie die Betaversion von Network Analytics verwendet haben, müssen Sie zunächst die alten {{site.data.keyword.security-advisor_short}}-Komponenten deinstallieren, bevor Sie die neuen Komponenten installieren können. Stellen Sie sicher, dass Sie diesen Prozess für jeden Cluster wiederholen, der Servicekomponenten enthält.
{: shortdesc}

1. Melden Sie sich bei {{site.data.keyword.Bluemix_notm}} an.

  ```
  ibmcloud login -a https://api.us-south.ibm.cloud.com --sso
  ```
  {: pre}

2. Listen Sie alle Cluster in Ihrem Konto auf, um den Namen des Clusters abzurufen.

  ```
  ibmcloud ks clusters
  ```
  {: pre}

3. Geben Sie den Cluster in der Befehlszeilenschnittstelle als Ziel an.

  ```
  ibmcloud ks cluster-config <clustername_oder_-ID>
  ```
  {: pre}

4. Definieren Sie den Pfad zur lokalen Kubernetes-Konfigurationsdatei als Umgebungsvariable. Beispiel:

  ```
  export KUBECONFIG=/Users/<benutzername>/.bluemix/plugins/container-service/clusters/<clustername>/kube-config-prod-dal10-<clustername>.yml
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
