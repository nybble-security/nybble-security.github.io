<center>![Sekoia Logo](../img/connectors/sekoia.png){: style="width:800px;"}</center>
# Configurer un connecteur Sekoia

## Overview
Suivez cette page pour envoyer vos alertes Sekoia.IO vers Nybble Hub.  
Nous allons configurer un connecteur dans le Hub, un Playbook Sekoia et le SSO pour les analystes.

## 1. Sekoia: Configuration SSO (partie 1/2)
!!! note
    Documentation officielle Sekoia : https://docs.sekoia.io/getting_started/sso/openid_connect/

Nos analysts font appel au SSO Nybble pour se connecter à votre communauté Sekoia.  
Les noms d'utilisateurs finissent par `@nybble.bzh`. C'est le domaine qu'il faut valider.

Après validation, copier la `Single Sign-on URL` (de forme `https://app.sekoia.io/user/oidc/<some_uuid>`), nous allons l'utiliser dans la création du connecteur côté Nybble. 

Le SSO requiert également un rôle spécifique, restreint, pour nos analystes.  
Créez un rôle nommé `nybble_analysts` avec les permissions suivantes :

![sekoia custom role](../img/connectors/sekoia-custom-role.png){: style="width:500px;"}


## 2. Connecteur Nybble Hub

1. Se connecter au Nybble Hub avec vos identifiants
2. Aller dans Paramètres > Connecteurs
3. Ajoutez un connecteur Sekoia avec les paramètres suivants :

    | Champ | Explication | Valeur usuelle |
    | ------ | ----------- | -------- |
    | Display Name | pour l'affichage dans le hub | `sekoia` |
    | SSO Login URL | URL SSO (chapitre précédent) | `https://app.sekoia.io/user/oidc/<some_uuid>` |

4. Cliquez sur Sauvegarder

    !!! warning
        A ce stade, le mot de passe du connecteur sera généré et disponible dans une fenêtre contextuelle.  
        Assurez-vous de copier et de stocker ce mot de passe dans un endroit sécurisé car il ne sera plus affiché !  
        Vous pouvez toujours le réinitialiser par la suite, mais vous devrez mettre à jour tous les webhooks avec la nouvelle valeur.

5. Téléchargez le fichier de configuration SSO.

## 3. Sekoia: Configuration SSO (partie 2/2)

1. Terminez la configuration SSO dans Sekoia avec les valeurs suivantes :

    | Champ | Explication | Valeur |
    | ------ | ----------- | -------- |
    | Create account | Pour créer les comptes Nybble à la volée | `Yes` |
    | Role by default | Rôle spécifique | `nybble_analysts` |
    | SSO configuration > Authentication Provider URL | URL SSO Nybble | `https://auth.nybble-analytics.io/` |
    | SSO configuration > Client ID | clientID (connecteur Hub) | voir le fichier téléchargé |
    | SSO configuration > Client Secret | client Secret (connecteur Hub) | voir le fichier téléchargé |

2. Cliquez sur Save Changes.

## 4. Sekoia : playbook

Nybble a développé une intégration Sekoia native :

- playbook template
- playbook action : send alert

### Account
1. Allez dans Playbooks -> Accounts -> new, et chercher Nybble
2. Remplissez le formulaire avec:

![nybble account in sekoia](../img/connectors/sekoia-connector-nybble-account.en.png){: style="width:600px;"}


### Playbook

!!! note
    Pour chaque alerte que vous envoyez à Nybble, contactez nos Professional Services  
    Ils vous confirmeront la bonne intégration de ces règles dans le Hub, notamment en ce qui concerne les règles personnalisées.

1. Allez dans Playbooks -> New Playbook -> use a template -> chercher "nybble"
2. Configurez l'étape `Alert Created` sur la partie Trigger Configuration
3. Selectionnez l'account nécessaire sur chaque étape du playbook
4. Sauvegardez et activez le playbook
