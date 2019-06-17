---

copyright:
  years: 2019
lastupdated: "2019-06-06"

---

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# Traitement des incidents
{: #troubleshooting}

Si vous rencontrez des problèmes lorsque vous utilisez {{site.data.keyword.security-advisor_long}}, les techniques suivantes peuvent vous aider à les résoudre et à obtenir de l'aide.
{: shortdesc}


## Aide et support
{: #getting-help-and-support}



Vous pouvez trouver de l'aide en recherchant des informations ou en posant des questions via un forum. Vous pouvez aussi ouvrir un ticket de demande de service. Lorsque vous utilisez les forums pour poser une question, prenez soin d'étiqueter cette dernière de façon à ce qu'elle soit vue par les équipes de développement {{site.data.keyword.cloud_notm}}.
{: shortdesc}

  * Si vous avez des questions d'ordre technique concernant {{site.data.keyword.security-advisor_short}}, postez votre question sur [Stack Overflow](https://stackoverflow.com/){: external}. Veillez à inclure les étiquettes `security-advisor` et `ibm-cloud`.

  * Posez toute question relative au service et aux instructions de mise en route sur le forum [dW Answers](https://developer.ibm.com/){: external}. Veillez à inclure les étiquettes `security-advisor` et `ibm-cloud`.


Pour plus d'informations sur l'obtention de support, voir [Comment obtenir le support dont j'ai besoin ?](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).


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
Avant d'installer l'une des offres Built-in Insights, vous devez installer Helm. Pour ce faire, référez-vous à la [documentation relative à l'intégration de Kubernetes Service](/docs/containers?topic=containers-helm).


## Problème connu : vous ne parvenez pas à créer un occurrence de solution personnalisée
{: #ts-custom-occurrence}

{: tsSymptoms}
Vous essayez de créer une occurrence de solution personnalisée, mais les informations ne s'affichent pas dans un navigateur et vous ne recevez aucun message d'erreur.

{: tsCauses}
La création de l'occurrence échoue car le nom que vous avez choisi est déjà utilisé.

{: tsResolve}
Choisissez un autre nom pour votre occurrence.

## Problème connu : l'image de connexion directe ne s'actualise pas
{: #ts-known-cant-edit}

{: tsSymptoms}
Lorsque vous téléchargez ou modifiez une image pour une connexion directe, elle ne s'affiche pas immédiatement.

{: tsCauses}
Le fait que les images ne s'affichent pas correctement est un problème connu.

{: tsResolve}
Pour voir l'image mise à jour, actualisez votre navigateur. Ceci affichera la nouvelle image dans le tableau de connexion directe.

