---
title: "Security is about data, oui mais pas que !"
description: "Si la data occupe une place de choix dans le processus de détection, il ne faut pas oublier l’humain qui maximise les capacités de détection, mène les investigations et répond aux incidents."
date: 2023-10-26 07:31:33 +0000
author: "Sébastien Lehuédé"
tags: [siem, data, detection, soc]
image: "/assets/img/blog/security-data-hero.webp"
---

L’article « Security is about data » de Ross Haleliuk aborde le sujet des Modern SIEM, de l’architecture data et explique en quoi la data joue un rôle essentiel dans la détection des menaces et attaques. Il ne faut pas oublier l’autre pilier de la sécurité : l’humain. Pour compléter les propos de Ross, je veux apporter ma vision de la place de la data dans la sécurité. Ce sont des points de vue issus du terrain.

Les recommandations et constatations qui suivent sont le résultat d’une dizaine d’années à travailler dans le domaine de la détection et réponse à incidents, à la fois du côté éditeur de solution SIEM, SOC interne ou MSSP.

## Pas n'importe quelle data

Selon Ross, toute donnée est bonne à prendre (Security, Finance, Marketing, …) et à stocker dans le Data Lake, car elle peut être utile pour la détection. En partie vrai. Il faut néanmoins s’assurer que ces données répondent à un besoin précis et ne viennent pas rajouter de faux positifs en cas de recherche ou de hunting.

## Pas de normalisation, pas de détection. Pas de détection, pas d'alerte. Pas d'alerte... pas d'alerte

La première étape consiste à normaliser les évènements de sécurité vers un même format, schéma. Les schémas fréquemment utilisés aujourd’hui sont : CEF (Common Event Format), ECS (Elastic Common schema), OSSEM (Open source Security Events Metadata), GraphQL. Le format dépend des capacités de l’outil SIEM et du SOC. L’objectif est de rendre immédiatement exploitables les logs collectés, et donc économiser du temps pour passer rapidement aux tâches d’analyse et d’investigation.

La solution est alors de normaliser les logs pour permettre au SIEM de déclencher des alertes et ensuite pour permettre à l’analyste de comprendre immédiatement l’alerte. Par exemple, un certain nombre d’informations sont indispensables pour un SOC : source_ip, destination_ip, user, outcome, event_id, event_type, source/destination_process, destination_url.

Le choix du schéma (qui est d’ailleurs parfois imposé selon l’éditeur du SIEM) constitue en lui même une possible contrainte, quand d’autres champs peuvent être utiles à la détection d’une menace spécifique par exemple.

La normalisation constitue donc la base d’un SIEM efficace. Elle doit être mûrement réfléchie en début de projet afin de ne pas devoir être modifiée par la suite. Elle doit être conçue par les équipes SOC / Incident Response, par des CyberAnalysts connaissant la data avec laquelle ils vont travailler.

## Enrichissement : analyste sans contexte, investigation complexe

Après le parsing et la normalisation, vient l’enrichissement. Cette étape vise à ajouter le maximum de contexte aux données. C’est ce contexte qui est ensuite utilisé par les règles de détection et l’analyste SOC / Incident Response lors de l’investigation.

L’enrichissement doit absolument être réalisé avant le stockage des logs et le processing par le moteur de corrélation. Ce n’est pas uniquement pour des raisons de temps d’intervention sur un incident ou d’optimisation, mais aussi pour ne pas perdre des logs importants pour lesquels le contexte n’était pas disponible au moment du parsing.

L’enrichissement peut se faire à partir de sources de données internes ou externes (elles peuvent être gratuites, payantes, des feeds, du machine learning, des données stockées…). Voici quelques exemples :

Interne : CMDB, LDAP Query, DHCP Lease, DNS PTR Record, Company site list, …

Externe : Onyphe, Virus Total, Threat Intel feeds, DNS Lookup, Alexa Top 1000, …

Grâce à l’enrichissement un analyste sera capable d’analyser et de comprendre plus rapidement la raison de l’alerte, il se concentre sur ce qui compte pour sa stratégie de hunt, et son investigation.

## Filtrage à la Sven Marquardt

Il est fréquent d’être confronté au raisonnement suivant : “J’active tous les logs en debug, comme ça je suis sûr de pouvoir détecter”, ou “je loggue toutes les requêtes web pour quelques微secondes tiens ? Je les garde dans mon SIEM”. Cela peut paraitre légitime mais conduit le plus souvent à l’effet inverse pour plusieurs raisons :

- Utilisation excessive des ressources du SIEM. Le CPU et la mémoire vont être utilisés pour parser, normaliser, enrichir des logs qui ne seront pas utilisés par la suite. Cela va entrainer une réduction des performances du SIEM.
- Augmentation du budget SIEM. Cela amène à devoir faire des arbitrages contre-productifs lorsqu’il faut choisir des logs à collecter, à cause du coût d’indexation pour le stockage.
- Pollution du Data Lake. L’analyste aura sans doute à filtrer plus d’éléments indésirables lors de ses investigations ou campagnes de hunting.

L’idée est d’être le plus restrictif possible en vérifiant à l’avance quels logs sont nécessaires avant d’envisager leur collecte, tout en gardant une marge de manœuvre pour les besoins futurs et imprévus.

Il est possible de s’appuyer sur des matrices qui référencent les types de logs nécessaires par tactiques et techniques du framework MITRE ATT&CK afin de ne collecter uniquement que le minimum nécessaire.

Astuce security “Used-case” offerte :

Cryptominer detection with CPU usage monitoring, à réaliser avec des métriques (si elles arrivent déjà dans votre SIEM) ou à travers des logs issus de scripts d’export et du déploiement d’agents.

## Security is about human

Maintenant que les données sont correctement formatées, stockées et correspondent à de vrais besoins, on peut parler de la place de l’humain. Les processus que l’on souhaite automatiser avec l’IA sont aujourd’hui en majorité gérés par un analyste.

## SOC Club : les règles

Le travail d’un analyste est de traiter une alerte afin de décider s’il s’agit d’un faux-positif ou d’une alerte qui présente un risque pour l’entreprise. Après investigation, il apporte de la précision sur la qualité de la règle, pour permettre son tuning ou son amélioration.

Pour les règles de détection existantes, en cas de faux-positif il doit comprendre les raisons pour lesquelles un évènement a généré l’alerte afin de l’éviter à l’avenir, et si l’alerte est légitime, comment la rendre plus précise et donc exploitable par ses collègues en cas d’incident similaire.

L’analyste doit également assurer un travail de veille et se tenir au courant des nouvelles vulnérabilités, des techniques d’attaque émergentes, et des stratégies de défense pour maintenir la pertinence du SIEM.

Dans tous les cas, un humain est en charge de cette partie qui peut prendre du temps, être répétitive, et mener à la fatigue de l’analyste… À quand le moment où il fantasme sur de la détection automatisée sans erreur ?

## Automatisation & AI : Not A Kind Of Magic

L’automatisation et l’IA sont vendues comme des outils magiques qui vont pouvoir répondre à 100% de vos besoins et vous affranchir des analystes. Nombreuses sont les entreprises séduites par ce pitch.

Il est indéniable que ce type d’outil aide les analystes dans leurs tâches, en assurant les parties les plus pénibles de l’analyse en proposant des actions d’administrateur système, ou en proposant des pistes plus rapides à explorer. Ils évitent les actions manuelles et répétitives.

L’automatisation à 100% est un risque que peu de décideurs sont prêts à prendre en raison des impacts possibles sur la production ou, à l’inverse, le fait de laisser passer une attaque.

Une fois encore, ces outils apportent une aide précieuse aux analystes, ils peuvent réduire le nombre d’erreurs et permettre de créer des automatismes dans les actions de réponse. Ils deviendront des alliés pour l’équipe SOC.

## Data Lake & Hunting, la pêche à la grenade ?

Le travail d’un analyste forensic ou d’un threat hunter requiert des compétences et des connaissances plus importantes que celles d’un analyste SOC (niveau 1). Il va créer ses propres signes de détection (IOA, IOB) et la qualité de ses détections dépend de sa capacité à poser les bonnes questions et utiliser les bons outils. C’est particulièrement vrai pour les activités de hunting.

Un trop grand nombre de données “polluantes” dans le Data Lake rendra le travail du hunter plus difficile et lui fera perdre du temps. C’est pour cela qu’il est important de maintenir un ensemble de données correctement normalisées, enrichies et filtrées.

L’intervention humaine, assisté par des outils d’IA ou non, est donc toujours nécessaire pour ce type d’activité. On considère que pour être efficace il faut que les données soient réellement utiles et correctement et rapidement exploitables grâce à une normalisation intelligente.

## SIEM is not dead, analysts ahead 🤘🏼

Comme dit en introduction, n’hésitez pas à réagir, apporter votre point de vue et vos retours d’expérience. C’est toujours intéressant d’avoir d’autres perspectives sur ces sujets qui évoluent régulièrement.

- ➡️ [LinkedIn](https://www.linkedin.com/company/nybble-security/)
- ➡️ [Contact](/index.html#contact)
