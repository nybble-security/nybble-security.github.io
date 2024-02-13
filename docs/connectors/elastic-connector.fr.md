# Configurer un Connecteur Elastic
!!! note
     Cette documentation convient uniquement aux clients Bring Your Own SIEM (BYOS).  
     Si vous êtes un client Elastic By Nybble, toutes les configurations sont déjà effectuées.

## Aperçu
En suivant cette page, vous configurerez la connexion entre votre SIEM et Nybble Hub.
Il se compose d'un point de terminaison côté Hub et d'un connecteur webhook côté Elastic.

## Côté Hub : connecteur
1. Connectez-vous à Nybble Hub en utilisant vos identifiants habituels
2. Accédez à Paramètres > Connecteurs
3. Ajoutez un connecteur Elastic puis remplissez le formulaire :

     | Champ | Explication | Valeur habituelle |
     | ------ | ----------- | -------- |
     | Nom d'affichage | nom à afficher lors de l'authentification et dans les configurations du hub | `elastic` |
     | URL Kibana | URL racine de Kibana, sera utilisée pour forger toutes les URL permettant d'accéder à votre SIEM depuis Hub | `https://contoso.kb.northeurope.azure.elastic-cloud.com:9243` |

4. Cliquez sur Enregistrer

!!! warning
     A ce stade, le mot de passe du connecteur sera généré et disponible dans une fenêtre contextuelle.  
     Assurez-vous de copier et de stocker ce mot de passe dans un endroit sécurisé car il ne sera plus affiché !  
     Vous pouvez toujours le réinitialiser par la suite, mais vous devrez mettre à jour tous les webhooks avec la nouvelle valeur.

## Côté Elastic : connecteur webhook
!!! note
     Cette étape nécessite des droits d'administrateur du côté d'Elastic.

1. Accédez à Stack Management -> Alertes et informations -> Connecteurs et cliquez sur « Créer un connecteur »
2. Sélectionnez le type `Webhook`
3. Remplissez les champs :

     | Champ | Explication | Valeur habituelle |
     | ------ | ----------- | -------- |
     | Nom du connecteur | nom d'affichage | `nybble` |
     | Paramètres/Méthode du connecteur | - | POST |
     | Paramètres du connecteur / URL | URL du point de terminaison des connecteurs nybble centraux | `https://connectors.nybble-analytics.io/conn/elastic` |
     | Authentification / Nom d'utilisateur, Mot de passe | authentification du connecteur | valeurs de `Côté Hub : connecteur` |
     | Authentification / En-tête HTTP | informations complémentaires, obligatoires | clé : `Content-Type` <br> Valeur : `application/json` |

4. Cliquez sur Enregistrer.

Les dernières étapes consisteront à utiliser ce connecteur sur toutes les actions de règles de sécurité, afin d'envoyer des alertes déclenchées aux services Nybble.  
Habituellement, cette étape est effectuée avec Nybble (Professional Services) en fonction du périmètre de détection.