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

# Vorteile vorintegrierter Services nutzen
{: #setup-services}

{{site.data.keyword.security-advisor_short}} wird mit einigen vorab gefüllten Karten geliefert, die Sie unterstützen können, wenn Sie eine Überwachung auf Sicherheitsrisiken und Bedrohungen ausführen.
{: shortdesc}

Für die folgenden Services erstellt {{site.data.keyword.security-advisor_short}} automatisch eine Karte:

* {{site.data.keyword.registrylong_notm}}
* {{site.data.keyword.cloudcerts_long_notm}}

Obwohl zur Erstellung der Verbindung zwischen {{site.data.keyword.security-advisor_short}} und den Services keine Schritte auszuführen sind, müssen Sie Instanzen der Services mit Informationen konfigurieren.


## Sicherheitslücken in Container-Images überwachen
{: #setup-images}

Mithilfe von {{site.data.keyword.registryshort_notm}} haben Sie Zugriff auf Vulnerability Advisor, womit die Images in Ihrer {{site.data.keyword.registryshort_notm}}-Instanz kontinuierlich auf mögliche Sicherheitsprobleme überprüft werden. Werden Probleme gefunden, werden Sie benachrichtigt und Sie haben die Möglichkeit, einen umfassenden Bericht in Ihrem {{site.data.keyword.security-advisor_short}}-Dashboard anzuzeigen.
{:shortdesc}

Weitere Informationen hierzu finden Sie unter [{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started).


### Vorbereitende Schritte
{: #setup-before}

Um die Registry verwenden zu können, müssen Sie sicherstellen, dass die folgenden Befehlszeilenschnittstellen und Plug-ins installiert sind:
* [{{site.data.keyword.cloud_notm}}-CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli)
* Plug-in 'Container-Registry'

  ```
  ibmcloud plugin install container-registry
  ```
  {: codeblock}


### Namensbereich erstellen
{: #setup-create-namespace}

1. Melden Sie sich über die Befehlszeilenschnittstelle (CLI) bei Ihrem Konto an.

  ```
  ibmcloud login --sso
  ```
  {: codeblock}

2. Melden Sie sich bei {{site.data.keyword.registryshort_notm}} an.

  ```
  ibmcloud cr login
  ```
  {: codeblock}

3. Erstellen Sie wahlweise einen Namensbereich. Sie können immer einen vorhandenen Namensbereich verwenden.

  ```
  ibmcloud cr namespace-add
  ```
  {: codeblock}

3. Kennzeichnen Sie ein Image mit einem Tag.

  ```
  docker tag <image>:<tag> <region>.icr.io/<namespace>/<image>:<tag>
  ```
  {: codeblock}

5. Übertragen Sie das Image mit einer Push-Operation.

  ```
  docker push <region>.icr.io/<namespace>/<image>:<tag>
  ```
  {: codeblock}


Nachdem Sie Images mit einer Push-Operation in Ihren {{site.data.keyword.registryshort_notm}}-Namensbereich übertragen haben, werden Informationen zu allen gefundenen Sicherheitslücken auf der Karte **Images mit Sicherheitslücken** im Service-Dashboard angezeigt. Sie können für bestimmte Images auch einen Drilldown durchführen, um weitere Informationen zu erhalten, z. B. eine Beschreibung aller identifizierten Sicherheitslücken und Konfigurationsprobleme.


## Zertifikate überwachen
{: #setup-certificates}

Wussten Sie, dass {{site.data.keyword.cloudcerts_long_notm}} die Überwachung und Verwaltung Ihrer SSL/TLS-Zertifikate unterstützen kann? Durch die Integration von {{site.data.keyword.cloudcerts_short}} und {{site.data.keyword.security-advisor_short}} können Sie rechtzeitig Alerts zu den Ablaufterminen Ihrer Zertifikate erhalten, wodurch Ausfälle von Services oder Anwendungen vermieden werden.
{:shortdesc}

In Abhängigkeit von den Ablaufdaten des Zertifikats, das Sie in {{site.data.keyword.cloudcerts_short}} hochladen, werden die Untersuchungsergebnisse 90 Tage, 60 Tage, 10 Tage oder 1 Tag vor Ablauf des Zertifikats im {{site.data.keyword.security-advisor_short}}-Dashboard angezeigt. Darüber hinaus erhalten Sie täglich Benachrichtigungen über abgelaufene Zertifikate. Die täglichen Benachrichtigungen beginnen an dem Tag, der auf den Tag des Zertifikatsablaufs folgt.

Zum Auslösen einer manuellen Aktualisierung können Sie versuchen, ein Zertifikat in Ihre {{site.data.keyword.cloudcerts_short}}-Instanz hochzuladen, das nach einem Tag abläuft. Bei erfolgreichem Import sehen Sie, dass der Key Risk Indicator (KRI) sowie Untersuchungsergebnisse im {{site.data.keyword.security-advisor_short}}-Dashboard angezeigt werden.

Weitere Informationen zu [{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager?topic=certificate-manager-getting-started) finden Sie in der Dokumentation.
{: tip}

### Zertifikat erstellen
{: #setup-create-cert}

Zum Erstellen eines selbst signierten Zertifikats, das in einem Tag abläuft, können Sie den folgenden OpenSSL-Befehl in Ihrem Terminal absetzen.

```
openssl req -x509 -newkey rsa:2048 -keyout key.pem -subj "/CN=myservice.com" -out server.pem -days 1 -nodes
```
{: codeblock}


### Zertifikat hochladen
{: #setup-upload-cert}

1. Suchen Sie im {{site.data.keyword.cloud_notm}}-Katalog nach "{{site.data.keyword.cloudcerts_short}}".
2. Geben Sie der Serviceinstanz einen Namen oder verwenden Sie den vorgegebenen Namen.
3. Klicken Sie auf **Erstellen**.
4. Klicken Sie auf **Zertifikat importieren**, um die Zertifikate Ihrer Organisation in {{site.data.keyword.cloudcerts_short}} zu importieren.

Nachdem Sie Ihre Zertifikate importiert haben, werden auf der Karte **Zertifikate** im {{site.data.keyword.security-advisor_short}}-Dashboard Informationen wie Ablaufzeiten und abgelaufene Zertifikate angezeigt. Wenn Sie auf die Karte klicken, erhalten Sie ausführlichere Informationen zu den Zertifikaten, z. B. zu welcher Serviceinstanz die Zertifikate gehören. Darüber hinaus werden alle Maßnahmen angezeigt, mit denen Sie die Sicherheitslücken schließen können.

Um die Benachrichtigungen zu stoppen, müssen Sie Ihr Zertifikat verlängern, das Zertifikat in {{site.data.keyword.cloudcerts_short}} hochladen und das abgelaufene Zertifikat löschen.
{: tip}
