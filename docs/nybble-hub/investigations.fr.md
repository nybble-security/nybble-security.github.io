# Investigations
!!! note
     Un profil client ou analyste L2 est requis pour suivre cette documentation

## Tableau des investigations

![Tableau des investigations](../img/nybble-hub/investigations/invest-main-table.png)
Vous avez ici un aperçu de toutes les investigations effectuées par Nybble.  
Par défaut, ce tableau est classé par heure de mise à jour, pour vous afficher en premier les investigations les plus récentes. Vous pouvez filtrer ce tableau en cliquant sur les icônes d'en-tête :

- ![Icône de filtre](../img/nybble-hub/investigations/filter-icon-ronde.png){style="position:relative;top:5px"} pour ouvrir une boîte de filtre avec les valeurs disponibles
- ![Icône de tri](../img/nybble-hub/investigations/sort-icon-ronde.png){style="position:relative;top:5px"} pour modifier l'ordre d'affichage

Vous pouvez cliquer sur le nom de l'investigation pour afficher ses détails.

## Détails de l'investigation

### Résumé
![Résumé de l'investigation](../img/nybble-hub/investigations/invest-summary.png)
Le panneau récapitulatif est divisé en 3 blocs :  
(1) Informations générales : vous donne des informations d'horodatage, le statut, la priorité et quel analyste L2 est en charge de l'investigation.  
(2) Principales conclusions : résumé de l’investigation avec informations clés. Plus de détails sur le rapport  
(3) Panneau "Terminé" : visible uniquement si une investigation est terminée, il vous donne le statut final et permet de télécharger le rapport détaillé.  

### Alertes associées
![Alertes associées](../img/nybble-hub/investigations/alerts.png)
Ce tableau récapitule les alertes qui ont été corrélées et étudiées. Généralement, plusieurs alertes sont corrélées en tant que scénario d'attaque.

### Diagramme Mitre Att&CK
![Diagramme d'attaque Mitre](../img/nybble-hub/investigations/mitre.png)
[Mitre Att&CK](https://attack.mitre.org/) est une base de connaissances sur les tactiques et techniques adverses. Il permet de représenter et de classer les observations faites pour mieux comprendre et répondre à la menace.  
Nybble classe chaque technique trouvée en utilisant cette définition et les représente sur un diagramme, basé sur un historique d'attaque :

- généralement, les tactiques de gauche sont considérées comme les premières étapes d'un scénario d'attaque (reconnaissance du réseau, outillage)
- les tactiques à droite sont considérées comme la fin d'un scénario d'attaque (avec pour objectifs finaux : impact ou exfiltration).

En bref, **plus les bons chiffres sont élevés, plus l'impact est élevé ; plus ils sont à droite, plus l'impact est important**.

### Historique
![Historique des alertes](../img/nybble-hub/investigations/history.png)
Cet onglet affiche un historique de toutes les actions effectuées par le client ou tout analyste Nybble dans le cadre de cette investigation.

### Commentaires
![Commentaires d'alerte](../img/nybble-hub/investigations/comments.png)
Vous pouvez lire ici les éventuels commentaires/interrogations rédigés par l'analyste en charge de l'investigation (le plus récent en premier).  
Vous pouvez également interagir en postant un commentaire/une question qui sera lu et répondu par Nybble.  
Habituellement, cela permet d'avancer dans l'investigation en fournissant des informations complémentaires sans envoyer plusieurs mails/appels, tout est résumé sur une seule page.