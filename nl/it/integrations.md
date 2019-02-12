---

copyright:
  years: 2018
lastupdated: "2018-12-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Integrazioni
{: #about}

Con {{site.data.keyword.security-advisor_long}}, puoi integrare le ricerche provenienti dai tuoi servizi IBM Cloud, dalle soluzioni partner o creare le tue integrazioni personalizzate.
{: shortdesc}


## Ricerche pre-integrate
{: #built-in}

Con {{site.data.keyword.security-advisor_long}}, puoi ottenere informazioni approfondite su potenziali problemi attraverso funzionalità di servizio integrate.
{: shortdesc}


Come preconfigurazione, {{site.data.keyword.security-advisor_short}} si integra con:

* Gestore certificati per le notifiche correlate alla scadenza e al ciclo di vita dei certificati.
* Vulnerability Advisor per i dettagli sulle vulnerabilità delle immagini e sui problemi di configurazione.

Per ulteriori informazioni, vedi [Utilizzo vantaggioso dei servizi pre-integrati](setup.html).

</br>

## Soluzioni partner
{: #partner}

Le soluzioni partner sono un modo per migliorare la sicurezza delle tue distribuzioni IBM Cloud creando un collegamento tra {{site.data.keyword.security-advisor_short}} e altri strumenti di sicurezza al di fuori di IBM Cloud.
{: shortdesc}

{{site.data.keyword.security-advisor_short}} è attualmente partner delle seguenti aziende:

### NeuVector
{: #neuvector}

[NeuVector](https://neuvector.com/) fornisce una piattaforma di sicurezza automatizzata altamente integrata per Kubernetes e Red Hat OpenShift che ti consente di monitorare e proteggere il traffico di rete del contenitore. In particolare, il traffico di rete est-ovest.

Con NeuVector, puoi rilevare minacce e violazioni della rete per prevenire attacchi delle tue applicazioni basate su contenitore. Tramite il monitoraggio, puoi prevenire gli exploit e i breakout rilevando le escalation dei privilegi root, le scansioni delle porte, le shell inverse e l'attività sospetta del file system nei tuoi contenitori ed host.

Per informazioni su come configurare la tua istanza di NeuVector con la procedura guidata di integrazione, vedi [Soluzioni partner](partners.html).

Sei un partner e sei interessato a integrare la tua soluzione con {{site.data.keyword.security-advisor_short}}? Raggiungi il nostro team contattando Matt Ward all'indirizzo wardm@us.ibm.com.
{: tip}

</br>

## Integrazioni personalizzate
{: #custom}

Potresti già disporre di uno strumento di sicurezza da cui dipendi. Puoi integrare tale strumento con il dashboard {{site.data.keyword.security-advisor_short}} in modo che tutte le tue informazioni sulla sicurezza siano centralizzate in un unico posto.
{: shortdesc}

**Integrazione dei tuoi strumenti con la GUI**

Utilizzando la GUI di {{site.data.keyword.security-advisor_short}}, puoi aggiungere ai segnalibri i tuoi strumenti di sicurezza interni ed esterni in modo da avere un solo punto di accesso a qualsiasi cosa dal dashboard {{site.data.keyword.security-advisor_short}}. Ad esempio, potresti aggiungere ai segnalibri PagerDuty.

**Integrazione dei tuoi strumenti con l'API**

Con l'API Findings, puoi integrare le ricerche provenienti dai tuoi strumenti di sicurezza personalizzati nel dashboard {{site.data.keyword.security-advisor_short}}. Questo potrebbe essere uno strumento personalizzato o di un partner che genera un avviso o una ricerca di sicurezza che supporta anche l'interazione basata su API.

**Inizia subito**

Per conoscere la procedura consigliata su come utilizzare le API e creare segnalibri tramite la GUI, vedi [Strumenti di sicurezza personalizzati](/docs/services/security-advisor/custom.html).

</br>


## Analisi di rete (anteprima)
{: #network-analytics}

Con l'anteprima dell'analisi di rete, puoi rilevare le minacce monitorando le comunicazioni del tuo cluster {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Monitorando le comunicazioni del cluster per comunicazioni sospette da e verso i tuoi cluster, puoi sfruttare la threat intelligence offerta da IBM X-Force per contrassegnare eventuali problemi potenziali. Quando viene identificata una comunicazione sospetta, viene emessa una ricerca che puoi esaminare e valutare.

Per informazioni più dettagliate sull'analisi di rete, vedi [Analisi di rete](network-analytics.html).
{: tip}

</br>
</br>
