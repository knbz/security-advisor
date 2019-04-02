---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-14"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

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


# Integrazioni
{: #integrations}

Con {{site.data.keyword.security-advisor_long}}, puoi integrare altre informazioni approfondite pre-integrate, soluzioni partner o creare le tue integrazioni personalizzate.
{: shortdesc}


## Ricerche pre-integrate
{: #integrate-built-in}

Con {{site.data.keyword.security-advisor_long}}, puoi ottenere informazioni approfondite su potenziali problemi attraverso funzionalità di servizio integrate.
{: shortdesc}


Come preconfigurazione, {{site.data.keyword.security-advisor_short}} si integra con:

* Gestore certificati per le notifiche correlate alla scadenza e al ciclo di vita dei certificati.
* Vulnerability Advisor per i dettagli sulle vulnerabilità delle immagini e sui problemi di configurazione.

Per ulteriori informazioni, vedi [Utilizzo vantaggioso dei servizi pre-integrati](/docs/services/security-advisor?topic=security-advisor-setup-services).

</br>

## Integrazioni partner
{: #integrate-partner}

Le integrazioni partner sono un modo per migliorare la sicurezza delle tue distribuzioni {{site.data.keyword.cloud_notm}} creando una connessione tra {{site.data.keyword.security-advisor_short}} e altri strumenti di sicurezza che collaborano con IBM per garantire un'esperienza utente senza eguali.
{: shortdesc}

Al momento i partner di {{site.data.keyword.security-advisor_short}} includono Neuvector e Twistlock.

Sei un partner e sei interessato a integrare la tua soluzione con {{site.data.keyword.security-advisor_short}}? Raggiungi il nostro team contattando Matt Ward all'indirizzo wardm@us.ibm.com.
{: tip}

### NeuVector
{: #integrate-neuvector}

[NeuVector](https://neuvector.com/) fornisce una piattaforma di sicurezza automatizzata altamente integrata per Kubernetes e Red Hat OpenShift che ti consente di monitorare e proteggere il traffico di rete del contenitore. In particolare, il traffico di rete est-ovest.

Con NeuVector, puoi rilevare minacce e violazioni della rete per prevenire attacchi delle tue applicazioni basate sul contenitore. Tramite il monitoraggio, puoi prevenire gli exploit e i breakout rilevando le escalation dei privilegi root, le scansioni delle porte, le shell inverse e l'attività sospetta del file system nei tuoi contenitori ed host.

Per assistenza nella configurazione della tua istanza NeuVector, vedi [Soluzioni partner](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-neuvector).


### Twistlock
{: #integrate-twistlock}

Twistlock ha la capacità unica di evitare ogni rischio durante l'SDLC evitando lo sviluppo di immagini vulnerabili nel tuo ambiente. L'applicazione della politica personalizzata e automatizzata offre il controllo completo in ogni fase del ciclo di vita dell'applicazione. Twistlock Intelligence Stream origina e aggrega le informazioni sulla vulnerabilità direttamente da 30+ progetti di upstream, fonti commerciali e ricerche brevettate da Twistlock Labs.

Con l'obiettivo di avere dei dati più precisi disponibili per coprire tutti i livelli del tuo stack, avrai una visibilità accurata e il tasso più basso di falsi positivi.Twistlock combina questi dati con le informazioni relative alle tue distribuzioni reali come ad esempio i contenitori che vengono esposti su internet, eseguiti con privilegi elevati e che hanno altre migrazioni di sicurezza in atto, per cui puoi sempre vedere quali vulnerabilità sono le più critiche nel tuo ambiente specifico.

Per assistenza nella configurazione della tua istanza Twistlock, vedi [Soluzioni partner](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-twistlock).
</br>


## Integrazioni personalizzate
{: #integrate-custom}

Potresti già disporre di uno strumento di sicurezza da cui dipendi. Puoi integrare tale strumento con il dashboard {{site.data.keyword.security-advisor_short}} in modo che tutte le tue informazioni sulla sicurezza siano centralizzate in un unico posto.
{: shortdesc}

### Creazione di una connessione diretta
{: #create-direct-connect}

Utilizzando la GUI di {{site.data.keyword.security-advisor_short}}, puoi aggiungere ai segnalibri i tuoi strumenti di sicurezza interni ed esterni in modo da avere un solo punto di accesso a qualsiasi cosa dal dashboard {{site.data.keyword.security-advisor_short}}. Ad esempio, potresti aggiungere ai segnalibri PagerDuty.

### Integrazione dei tuoi strumenti con l'API
{: #integrate-tools-api}

Con l'API Findings, puoi integrare le ricerche provenienti dai tuoi strumenti di sicurezza personalizzati nel dashboard {{site.data.keyword.security-advisor_short}}. Questo potrebbe essere uno strumento personalizzato o di un partner che genera un avviso o una ricerca di sicurezza che supporta anche l'interazione basata sull'API.

## Built-in Insights
{: #integrate-insights}

Con Built-in Insights, puoi rilevare dei problemi potenziali monitorando in modo continuo i tuoi log di account e cluster. Monitorando il traffico di rete e l'attività dell'utente, puoi assicurarti che le risorse di {{site.data.keyword.cloud_notm}} rimangano protette.
{: shortdesc}

**Network Insights (beta)**

Con Network Insights (beta), puoi monitorare ed analizzare la comunicazione di rete del cluster, sia in entrata che in uscita, tra il tuo cluster Kubernetes ed entità esterne. Utilizzando la threat intelligence e il rilevamento delle anomalie integrati, il servizio può identificare attacchi di ricognizione e asset potenzialmente compromessi. Per ulteriori informazioni, vedi [Network Insights](/docs/services/security-advisor?topic=security-advisor-network).

**Activity Insights (anteprima)**

Con Activity Insights (anteprima), puoi monitorare in modo continuo i tuoi log del Programma di traccia dell'attività {{site.data.keyword.cloud_notm}} per identificare attività non autorizzata o sospetta effettuata da utenti o applicazioni utilizzando i pacchetti di regole. Puoi utilizzare i pacchetti di regole forniti dal servizio che si basano sulle procedure consigliate per la sicurezza o puoi personalizzare delle regole che si adattano ai tuoi bisogni. Per ulteriori informazioni, vedi [Activity Insights](/docs/services/security-advisor?topic=security-advisor-activity).
