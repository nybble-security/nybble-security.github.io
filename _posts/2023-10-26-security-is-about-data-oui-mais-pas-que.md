---
title: "Security is about data, oui mais pas que !"
description: "Si la data occupe une place de choix dans le processus de détection, il ne faut pas oublier l’humain qui maximise les capacités de détection, mène les investigations et répond aux incidents."
date: 2023-10-26 07:31:33 +0000
author: "Sébastien Lehuédé"
tags: [siem, data, detection, soc]
image: "/assets/img/blog/security-data-hero.webp"
---

L’article « [Security is about data](https://ventureinsecurity.net/p/security-is-about-data-how-different) » de Ross Haleliuk aborde le sujet des Modern SIEM, de l’architecture technique et des nouveaux types d’organisation pour tirer le maximum de profit de cet outil. Il a été largement partagé par communauté cyber mais n’aborde que très rapidement les sujets de la mise en forme de la donnée et l’utilisation de celle-ci par des humains (Analystes SOC & Forensic, équipes techniques, responsable compliance, …). C’est justement sur ces sujets que nous sommes experts chez Nybble et sur lesquels nous allons apporter plus de précision avec cet article de blog, en rebondissant sur celui de Ross.

Les recommandations et constatations qui suivent sont le résultat d’une dizaine d’années à travailler au sein de différents SOC et équipes de sécurité. Ils peuvent refléter un avis personnel et il se peut également que certains outils ou certaines méthodes de travail échappent à notre connaissance. C’est pourquoi nous serons très heureux de débattre de tous ces sujets et apprendre avec vous !

## Pas n'importe quelle data

Selon Ross, toute donnée est bonne à prendre (Security, Finance, Marketing, …) et à stocker dans le même Data Lake. De cette façon elles sont utilisables par différents outils qui n’ont plus qu’à venir se brancher sur le Data Lake. Dans la vraie vie, si on veut mettre en place et gérer efficacement les différents scénarios de détection d’un SIEM, il ne suffit pas de remplir un Data Lake d’évènements bruts et laisser la machine faire. Plusieurs étapes plus ou moins complexes sont obligatoires avant que le SIEM puisse réellement apporter de la valeur !

## Pas de normalisation, pas de détection. Pas de détection, pas d'alerte. Pas d'alerte... pas d'alerte

![mission-cleopatre-bisou](/assets/img/blog/2023-10-26-security-is-about-data-oui-mais-pas-que/asterix-et-obelix-mission-cleopatre-gif-bisou.gif)

La première étape consiste à normaliser les évènements de sécurité vers un même format, schéma. Les vendeurs de produits ou services (Firewall, Proxy, EDR, Application SaaS, …) ont, pour la plupart, des capacités de logging qui génèrent des évènements avec l’utilisation des solutions et notamment des évènements lié à la sécurité (Authentification, blocage d’accès, gestion des permissions, lancement de processus, accès web, …). Sur ce point chaque vendeur travaille de son côté dans son propre écosystème, sans se soucier des personnes et outils qui auront la nécessité d’utiliser cette donnée. Le résultat est que des dizaines de formats de logs existent, que rien n’est rationalisé (parfois même au sein d’une solution d’un vendeur) et que sans pré-traitement ces données sont inutilisables.

La solution est alors de normaliser les logs pour permettre au SIEM de déclencher des alertes et ensuite aider l’analyste sécurité à investiguer facilement, efficacement et rapidement en cas d’incident. Ce travail minutieux doit être fait pour chacune des sources de logs, avec dans le temps des ajustements à réaliser pour répondre aux besoins des analystes SOC et aux évolutions du schéma choisi.

Le choix du schéma (qui est d’ailleurs parfois imposé selon l’éditeur du SIEM) constitue en lui même un point d’attention particulier. Il doit être évolutif, personnalisable, compréhensible et surtout être utilisé par une communauté suffisamment importante afin qu’il soit implémenté sur les solutions tierces qui s’interfacent avec le SIEM. Comme pour les formats de log, chaque éditeur de SIEM dispose de son propre schéma (CEF, LEEF, ECS, …) qui convient à son écosystème.

La normalisation constitue donc la base d’un SIEM efficace. Elle doit être mûrement réfléchie en début de projet car les implications sont nombreuses et la correction a posteriori est difficile.

## Enrichissement : analyste sans contexte, investigation complexe

![subrote-hackers](/assets/img/blog/2023-10-26-security-is-about-data-oui-mais-pas-que/subrote-hackers.gif)

Après le parsing et la normalisation, vient l’enrichissement. Cette étape vise à ajouter le maximum de contexte à un log en fonction des informations de base qu’il contient.

L’enrichissement doit absolument être réalisé avant le stockage des logs et le processing par le moteur de règles/corrélation du SIEM. Pourquoi ? Car cela représente une énorme opportunité pour améliorer ses capacités de détection d’un côté et réduire le nombre de faux-positifs de l’autre.

L’enrichissement peut se faire à partir de sources de données internes ou externes (elles peuvent être stockées dans le Data Lake pour réutilisation et mise à jour). Une liste non exhaustive :

<ins>Interne :</ins> CMDB, LDAP Query, DHCP Lease, DNS PTR Record, Company site list, …

<ins>Externe :</ins> Onyphe, Virus Total, Threat Intel feeds, DNS Lookup, Alexa Top 1000, …

Grâce à l’enrichissement un analyste sera capable d’analyser et de comprendre plus rapidement la raison de déclenchement d’une alerte. Lors du tri et de la catégorisation il sera en mesure de prendre plus sereinement une décision sur la légitimité de celle-ci. Dernier avantage et pas des moindres, on lui évite de faire des allers retours sur plusieurs plateformes afin d’obtenir ces informations à la main et on réduit ainsi l’alerte fatigue.

## Filtrage à la Sven Marquardt

![ziekenhuisbal-steward](/assets/img/blog/2023-10-26-security-is-about-data-oui-mais-pas-que/ziekenhuisbal-steward.gif)

Il est fréquent d’être confronté au raisonnement suivant : “J’active tous les logs en debug, comme ça je suis sur de ne rien louper et d’avoir toutes les informations en cas d’investigation ! 👍🏼”.

Cela peut paraitre légitime mais conduit le plus souvent à l’effet inverse pour plusieurs raisons :

- Utilisation excessive des ressources du SIEM. Le CPU et la mémoire vont être utilisés pour parser, normaliser et enrichir des logs inutiles, affectant le run des règles de détection qui va subir des timeouts.
- Augmentation du budget SIEM. Cela amène à devoir faire des arbitrages contre-productifs lorsqu’il faut ajouter de nouvelles sources de log qui pourraient être utiles.
- Pollution du Data Lake. L’analyste aura sans doute à filtrer plus d’éléments indésirables lors de ses investigations, ce qui aurait pu être fait en amont et sans gâcher de ressources (CPU, Mémoire, Disque).

L’idée est d’être le plus restrictif possible en vérifiant à l’avance quels logs sont nécessaires avant même de mettre en place une nouvelle règle de détection et de les activer à la demande. En réalité dans la majorité des cas il sera possible de filtrer les logs indésirables dès leur arrivée sur le collecteur.

Il est possible de s’appuyer sur des matrices qui référencent les types de logs nécessaires par tactique et technique MITRE ATT&CK ([Exemple avec les EventID Windows](https://github.com/mdecrevoisier/EVTX-to-MITRE-Attack)).

<ins>Astuce security “Used-case” offerte :</ins>

Cryptominer detection with CPU usage monitoring, à réaliser avec des métriques (si elles arrivent déjà dans votre SIEM) ! 👍🏼

## Security is about human

Maintenant que les données sont correctement formatées, stockées et correspondent à de vrais besoins, passons à l’élément le plus important d’un SOC, l’humain 👩🏻‍💻👨🏾‍💻

## SOC Club : les règles

![tyler-durden](/assets/img/blog/2023-10-26-security-is-about-data-oui-mais-pas-que/tyler-durden.gif)

Le travail d’un analyste est de traiter une alerte afin de décider s’il s’agit d’un faux-positif ou d’un véritable incident, du moins en partie. En effet son travail ne s’arrête pas là.

Pour les règles de détection existantes, en cas de faux-positif il doit comprendre les raisons pour lesquelles la règle s’est déclenchée et apporter les améliorations nécessaires pour éviter que cela ne se reproduise. Il peut s’agir de cas particuliers à whitelister directement sur la règle ou de contexte supplémentaire à apporter sur les logs afin d’être plus précis dans la requête.

L’analyste doit également assurer un travail de veille et se tenir au courant des nouvelles vulnérabilités et failles, de suivre l’évolution des différents groupes d’attaquants pour développer de nouvelles règles de détection, ou au moins vérifier si celles-ci n’ont pas été déjà été écrites en SIGMA. 2 exemples connus sont les attaques Sunburst et PrintNightmare, où plus de 5 règles de détection ont été mises à disposition de la communauté en seulement 4h.

Dans tous les cas, un humain est en charge de cette partie qui peut prendre du temps, être répétitive et amener au sentiment que toutes ces actions ne servent finalement à rien. Il est donc important de partager le travail de création, correction et d’amélioration de ces règles, cela permet d’aller plus loin en terme de capacités de détection et renforce le sentiment d’utilité des analystes. Le renforcement de ce sentiment allié à la réduction de l’alerte fatigue constitue un puissant atout pour garder une équipe efficace et motivée !

## Automatisation & AI : Not A Kind Of Magic

![freddy-mercury-iwtbf](/assets/img/blog/2023-10-26-security-is-about-data-oui-mais-pas-que/freddy-mercury-iwtbf.gif)

L’automatisation et l’IA sont vendues comme des outils magiques qui vont pouvoir répondre à 100% de vos alertes et réduire votre taux de faux-positifs à 0%, mais dans les faits c’est un peu plus compliqué.

Il est indéniable que ce type d’outil aide les analystes dans leurs tâches, en assurant les parties les plus simples du traitement, en ajoutant du contexte aux alertes ou en redirigeant les alertes directement vers les équipes concernées. Mais elles ne sont pas encore capables de remplacer totalement un être humain.

L’automatisation à 100% est un risque que peu de décideurs sont prêts à prendre en raison des impacts que cela peut avoir. On parle bien sur de l’IP Microsoft qui s’est glissée dans une liste de Threat Intelligence, qui sera automatiquement ajoutée à une règle Firewall Block et qui empêchera l’accès à Teams pour toute l’entreprise durant toute une matinée avant qu’on ne trouve l’origine de la panne.

Une fois encore, ces outils apportent une aide précieuse aux analystes, ils peuvent réduire le nombre de tâches fastidieuses qu’ils ont à accomplir mais pas complètement les remplacer.

## Data Lake & Hunting, la pêche à la grenade ?

![freddy-mercury-iwtbf](/assets/img/blog/2023-10-26-security-is-about-data-oui-mais-pas-que/ezgif.com-crop.gif)


Le travail d’un analyste forensic ou d’un threat hunter requiert des compétences et des connaissances précises. Il ne s’agit pas de lancer des actions et requêtes au hasard sur un Data Lake et croiser les doigts pour espérer débusquer un acteur malveillant qui se serait caché pendant des semaines.

Un trop grand nombre de données “polluantes” dans le Data Lake rendra le travail du hunter plus difficile, et amènera probablement à l’effet “pêche à la grenade” afin d’espérer avoir la moindre touche avant un découragement total.

L’intervention humaine, assisté par des outils d’IA ou non, est donc toujours nécessaire pour ce type de mission qui nécessite un vrai savoir faire. Cette mission est extrêmement valorisante pour les analystes et hunters en cas de succès !

## SIEM is not dead, analysts ahead 🤘🏼

Comme dit en introduction, n’hésitez pas à réagir, apporter votre point de vue et vos retours d’expérience !

- ➡️ [LinkedIn](https://www.linkedin.com/company/nybble-security/)
- ➡️ [Contact](/index.html#contact)
