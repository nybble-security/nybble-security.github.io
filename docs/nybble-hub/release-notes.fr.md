# Nybble Hub : Notes de version

## Release 2024.04.01
- L'intégration avec Sekoia.IO est désormais disponible globalement !
- Alertes triées par SLA au lieu de la sévérité
- Ajout de notifications par mail configurables dans la plateforme d'enregistrement
- Améliorations UI/UX sur les tableaux. 

## Release 2024.02.01
- BETA : Algorithme de scoring pour analystes
- Ajout de notifications par mail configurables via le profil utilisateur
- Correction d'un bug sur l'affichage du RAW Event dans une alerte

## Release 2024.01.01
- Amélioration globale des performances et de l'expérience utilisateur : moins de rafraichissements, gestion du cache
- Confirmation des actions via une notification discrète en bas à gauche de l'écran
- Correction d'un problème sur la statistique annuelle

## Release 2023.12.01 - Pacu
- Rebranding de la page d'accueil du Hub, suivant les nouvelles directives de style Nybble
- Afficher les informations d'inscription dans votre profil
- Alertes : des données contextuelles supplémentaires provenant des clients sont affichées côté wiki.
- Plateforme d'inscription & dojo :
     - historique complet de votre inscription et de vos tests
     - tentatives de tests
- mise à jour globale des composants internes et améliorations de la sécurité

## Release 2023.10.01 - Glaucus atlanticus

- BETA PRIVÉE : processus d'inscription pour les Nybblers :
     - saisir toutes les informations requises (entreprise, personnel, références)
     - réussir les tests d'analyste ou de Hunter (ou les deux !)
     - et devenez Nybbler 💪 💵
- mise à jour globale de la documentation et changement de style, suivant les nouvelles directives de style Nybble

## Release 2023.09.01 - Dugong

- 1ère version du renvoi des résultats d'alerte aux MSSP (via TheHive)
- Alertes : améliorations suite aux retours du Hacktoberfest CTF
- Campagnes de Threat Hunting : améliorations suite aux retours du Hacktoberfest CTF
- La langue est désormais déterminée par le navigateur par défaut
- mise à jour globale des composants internes

## Release 2023.06.01 - Sunda Colugo

- Un Nybbler peut désormais être à la fois analyste et Hunter
- vitesse améliorée du connecteur elastic

## Release 2023.05.01 - Dumbo Octopus

- le client peut désormais télécharger une configuration logstash personnalisée pour déployer l'agent central (Elastic By Nybble)
- mise à jour globale des composants internes

## Release 2023.02.01 - Babiroussas

- correction d'un problème d'affichage sur les détails de l'enquête
- Alertes : cliquer sur le champ Elastic ouvre le SIEM avec un filtre précis

## Release 2023.01.01 - Narval

- Prise en charge officielle d'Elastic (Bring your Own SIEM)

## Release 2022.12.01 - Cirrocumulus

- Édition mobile Nybble Hub : toutes les vues sont désormais optimisées pour l'affichage mobile

## Release 2022.11.01 - Cumulonimbus

- Implémentation d'APIKEYs par utilisateur : vous pouvez interagir avec notre API à partir d'outils tiers
- Correction des problèmes de téléchargement après la mise en œuvre du SSO

## Release 2022.10.01 - Altocumulus

- L'authentification unique 🔐 est disponible dans le monde entier :
     - les clients, les analystes et les Hunters peuvent désormais accéder à la fois aux SIEM et au Hub en utilisant les mêmes informations d'identification et en se connectant une seule fois par navigateur.
     - amélioration de la sécurité en exigeant une authentification à 2ème facteur (MFA)
     - réinitialisation du mot de passe en recevant un lien temporaire par mail
- résolution d'un problème lors du téléchargement de la configuration

## Release 2022.09.01 - Cirrostratus

  - correction d'un bug d'affichage de l'enquête
  - révision et amélioration du code qualité interne
  - 1ère version des guides utilisateurs

## Release 2022.08.01 - Nimbostratus

  - menu de paramètres pour les clients, pour télécharger les fichiers de configuration
  - pages wiki d'alertes : les analystes peuvent contribuer en postant une suggestion d'édition.
  - ajout de la langue française
  - résolution des problèmes CSS sur les petits écrans
  - les utilisateurs peuvent désormais modifier les informations de leur profil
  - diverses améliorations basées sur les retours du FIC
  - routine de mise à niveau des composants internes

## Release 2022.07.01 - Stratocumulus
 
Modules de base :
  
  - statistiques et tableaux de bord pour le client
  - modules de campagne de chasse et de rapport
  - interface de gestion des alertes
  - interface d'enquête pour les clients
  - interface d'administration