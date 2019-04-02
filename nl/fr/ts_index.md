---

copyright:
  years: 2019
lastupdated: "2019-02-18"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# Traitement des incidents
{: #troubleshooting}

Si vous rencontrez des problèmes lorsque vous utilisez {{site.data.keyword.security-advisor_long}}, les techniques suivantes peuvent vous aider à les résoudre et à obtenir de l'aide.
{: shortdesc}


## Aide et support
{: #getting-help-and-support}



Vous pouvez trouver de l'aide en recherchant des informations ou en posant des questions via un forum. Vous pouvez aussi ouvrir un ticket de demande de service. Lorsque vous utilisez les forums pour poser une question, prenez soin d'étiqueter cette dernière de façon à ce qu'elle soit vue par les équipes de développement {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

* Si vous avez des questions d'ordre technique concernant {{site.data.keyword.security-advisor_short}}, postez vos questions sur <a href="http://stackoverflow.com/" target="_blank">Stack Overflow <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. Veillez à inclure les étiquettes `security-advisor` et `ibm-cloud`.
* Posez toute question relative au service et aux instructions de mise en route sur le forum <a href="https://developer.ibm.com/" target="_blank">dW Answers <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. Veillez à inclure les étiquettes `security-advisor` et `ibm-cloud`.

Pour plus d'informations sur l'obtention de support, voir [Comment obtenir le support dont j'ai besoin ?](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).


## Vous ne parvenez pas à créer une occurrence de solution sur mesure
{: #ts-custom-occurrence}

{: tsSymptoms}
Vous essayez de créer une occurrence de solution personnalisée, mais les informations ne s'affichent pas dans un navigateur et vous ne recevez aucun message d'erreur.

{: tsCauses}
Il s'agit d'un problème connu. La création de l'occurrence échoue car le nom que vous avez choisi existe déjà.

{: tsResolve}
Choisissez un autre nom pour votre occurrence.


## Erreur : les espaces de nom "security-advisor-insights" sont interdits 
{: #ts-error-helm-install}

{: tsSymptoms}
Lorsque vous tentez d'installer Network ou Activity Insights, l'erreur suivante s'affiche : 

```
namespaces “security-advisor-insights” is forbidden: User “system:serviceaccount:kube-system:default” cannot get resource “namespaces” in API group “” in the namespace “security-advisor-insights”
```
{: screen}

{: tsCauses}
Le compte de service par défaut `kube-system` ne dispose pas d'un accès administrateur dans votre cluster. 

{: tsResolve}
Avant d'installer l'une des offres Built-in Insights, vous devez installer Helm. Pour ce faire, référez-vous à la [documentation relative à l'intégration de Kubernetes Service](/docs/containers?topic=containers-integrations#helm).


## Problème connu : certains résultats de Network Insights ne s'affichent pas 
{: #ts-network-analytics}

{: tsSymptoms}
Les types de résultat n'apparaissent pas tous lorsque vous recherchez Network Insights dans le tableau de bord ou l'API.

{: tsCauses}
Certains types de résultat de Network Insights fonctionnent uniquement avec IBM Cloud Kubernetes Service version 1.10 ou antérieure. 

{: tsResolve}
Utilisez Kubernetes Service version 1.10 ou antérieure. 
