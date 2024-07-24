# Configurer un Connecteur Elastic
!!! note
     Cette documentation convient uniquement aux clients Bring Your Own SIEM (BYOS).  
     Si vous êtes un client Elastic By Nybble, toutes les configurations sont déjà effectuées.

## En bref
Cette page décrit la configuration de l'interface entre le Nybble Hub et votre Elastic.  
Il s'agit de définir un connecteur et de configurer le SSO pour nos analystes.

!!! note
    Depuis la version 2024.06.01, le SSO peut être désactivé. Cependant, cela réduit les capacités d'analyse et de décision de notre communauté d'analyste.  
    Si vous souhaitez vous passer du SSO, omettez le chapitre `SSO Nybble`.

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


## Le SSO Nybble

Les analystes et chasseurs Nybble nécessitent un accès à votre SIEM afin de mener à bien leurs enquêtes.
Cet accès est limité :

  * espace dédié, avec accès à des fonctionnalités spécifiques, avec sa dataview spécifique
  * rôle restreint, où vous pouvez restreindre les index

De plus, nous connectons notre solution SSO à votre cluster, évitant ainsi la gestion de compte de votre côté. Notre SSO est compatible MFA et limité à un espace spécifique.

### Espace dédié Nybble (Space)
```json
POST kbn:/api/spaces/space
{
  "id": "nybble",
  "name": "Nybble",
  "description": "Dedicated space for Nybble detection rules, analysts and hunters.",
  "initials": "",
  "color": "#FFFFFF",
  "disabledFeatures": [
    "maps",
    "enterpriseSearch",
    "dev_tools",
    "advancedSettings",
    "indexPatterns",
    "filesManagement",
    "filesSharedImage",
    "savedObjectsManagement",
    "savedObjectsTagging",
    "osquery",
    "actions",
    "generalCases",
    "guidedOnboardingFeature",
    "rulesSettings",
    "maintenanceWindow",
    "stackAlerts",
    "fleetv2",
    "fleet",
    "monitoring",
    "siem",
    "securitySolutionCases",
    "logs",
    "infrastructure",
    "apm",
    "uptime",
    "observabilityCases",
    "slo",
    "dashboard",
    "ml",
    "graph",
    "visualize",
    "canvas"
  ],
  "imageUrl": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAAXNSR0IArs4c6QAABFFJREFUeF7tW9tx3DAMJN1BnIJyV43dRewu7GpyKSh2B6cMRUknUwCxeEieTHxfmQxFAoslsIDknI76Pb6f0jD8go57vc/QuoBFhx2UHt6K8yfI5jw8p5fvT9Ba56JjANBEf3boIBCOAUAT/VtEL+n1/uwMsPj4/gBYon8gC/YH4OFtEMPQW7BzQtwXgMc/T2nIP10A7JwL9gPAQ/0WsR1ZgANQojn+7i7p5dtFjGpE9JdckM/Yme9Tmb2e0DKKAcA7c0l5+L2A0dZu7/2fN14zoDBr/F1Pacg/piWUvoCqiAyALpIfD9U9S5MqN9HXlFQgf/QBsNzj1mAfCNsoalnV2tPA3AdAe1jdPI4FUWB2kigPgCdy7YG2vfzRB1QlDYDN4NtxFO20bIqKvqAqtwB4naeuQfk/zb5U8tI8z9VoYt8tAJosq5Wwde/+r5RVqhXWMog6BQLAkvnJw0DxIgGiZU9vPwiAskEMCyAhgvgfZE9KRDWgk2AEAIAIgZyPYgBjD1MFFPM78K7BznILvUmQEUS8DrCzII76LRg72NQRQkYWCNLTxQRrgu5cx1gp3Lv31XhsOMKVwjEfGAJjksLaaiAlPW0d7w1BNPlAsAtoh0v/fa39du2/t7235LzGYEG6LleIzwd1WDPOKe7qvzsDHBkANiuD0xcLZeczpVHYOKWSnexqI1dSQh62Z+4Sxd3fEPXb4ZE+jldUnuijV6EXhPX4jPEDb4dLNNCB6GyUJ/o3x3S6Yna6VpxbvoKVIJKwEDCQfZArVBNa/yqMTi9DUv4FrNgMWYzmEpW27Elg8Ofgb50JMG9XwOL8uCHR9kbc/RaQyHNWTKgAWJ3nANCKKCn67aB1Xu+yu16r7HK+GkInKY9xSPS9gZuuQw4YNvBZOgYEfv+AKhMBADlpWYLoTYa97tK99/CcTd0VSlEvTeXuUh6ydnVwGABCnbZGKqoj5EDI+VyrgNVAVK1ZcoE0WAm4/2VIOgOgExMUokjnhn8tIsvfoKDFACBJ1TEXrOYKYt0HPsKwsGp97mTzJIQMY6Z5M8R50WHjAg8IIQB8pvNeNThdWXsvcLTz5Qpxoy09E5YcQ78dLuj2EhaXoddNUBRAt2lybXO5fWUQLinn5xZE4ROZ6cuwNRic81xZsgLROk4ksE3m2HahpNMftoLTzzyAbGmItr4oED3HNwqUEGDzRAj5lK8QCgaAWijTbvsUS+HlxQn2SX1QFbIBoIkSjTD1/Y9HjIlUZ9WwiQF+GRoNQHFDVo+EszYGfAGg+PMXjmJt7xCk7bWM/iwGbIcoXgDQKtMgZAPAkv3b0EQz4J8DYC2oUC3R4/exADi6x6V+r94nhABg+yzPeAW+AMD/CpRVICsZ+58xgFZt3sQqjeSYQNiugHUYgSQqLRDInp3k6QNAA4Q05V0biYDgdHzJxVrl1F1PG27S6OM51H5Bju8DQMsITdQ5ZGcQgh2fj/sLWPgS2KwcngcAAAAASUVORK5CYII="
}
```

### Configuration Elastic

1. Depuis le Hub, récupérez le fichier yaml Elastic SSO disponible dans la section des connecteurs (Admin -> connecteurs) : ![Fichier de configuration Elastic SSO](../img/connectors/elastic-sso-config-file.png){:style="width:800px;"}
2. Importez le fichier yaml dans votre organisation Elastic (cloud) :
Déploiements -> `nom du déploiement` -> modifier -> elasticsearch -> gérer les paramètres utilisateur et les extensions

### Configuration de Kibana
Importez ce fichier yaml dans votre organisation Elastic, paramètres Kibana :

```yaml
xpack.security.authc.providers:
  saml.nybble:
    order: 0
    realm: nybble
    description: "Log in with Nybble Auth"
    hint: "Nybble Hunters and Analysts"
    icon: "data:image/svg+xml;base64,PHN2ZyBpZD0iTGF5ZXJfMSIgZGF0YS1uYW1lPSJMYXllciAxIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAzNjguOCA0MzQiPjxwYXRoIGQ9Ik0xNjAuMiwxMjMuOWwtMjguNiw0OC42LDI4LjYsNDguNmg1Ny4zbDI4LjYtNDguNi0yOC42LTQ4LjdaIiBzdHlsZT0iZmlsbDojMDA2N2VlIi8+PHBhdGggZD0iTTE2Mi43LDAsMTM2LjQsNDQuNmwyNi4zLDQ0LjdoNTIuNWwyNi4zLTQ0LjZMMjE1LjEsMFoiIHN0eWxlPSJmaWxsOiMwMDY3ZWUiLz48cGF0aCBkPSJNMzYuOSw4NS45bDE4LjUsNDguMyw1MS4yLDcuMywzMi43LTQxLjFMMTIwLjgsNTIuMSw2OS42LDQ0LjhaIiBzdHlsZT0iZmlsbDojMDA2N2VlIi8+PHBhdGggZD0iTTI1LjYsMjM3LjhsNDkuMywxNS43LDM3LjYtMzUuNi0xMS42LTUxLjJMNTEuNSwxNTEsMTMuOCwxODYuNVoiIHN0eWxlPSJmaWxsOiMwMDY3ZWUiLz48cGF0aCBkPSJNMTM3LjIsMzQxLjNsNDMtMjguOC00LjMtNTEuNi00Ny4zLTIyLjgtNDMsMjguOCw0LjMsNTEuNVoiIHN0eWxlPSJmaWxsOiMwMDY3ZWUiLz48cGF0aCBkPSJNMjg3LjcsMzE4LjYsMjkyLDI2N2wtNDMtMjguOC00Ny4zLDIyLjctNC4zLDUxLjUsNDMsMjguOFoiIHN0eWxlPSJmaWxsOiMwMDY3ZWUiLz48cGF0aCBkPSJNMzYzLjksMTg2LjZsLTM3LjUtMzUuNEwyNzcsMTY2LjksMjY1LjMsMjE4bDM3LjYsMzUuNkwzNTIuMiwyMzhaIiBzdHlsZT0iZmlsbDojMDA2N2VlIi8+PHBhdGggZD0iTTMwOC4zLDQ1bC01MS4yLDcuMi0xOC42LDQ4LjMsMzIuNyw0MS4xLDUxLjMtNy4yTDM0MSw4Ni4xWiIgc3R5bGU9ImZpbGw6IzAwNjdlZSIvPjxwYXRoIGQ9Ik0wLDM2OC4zSDE0LjlMMzcsNDAzLjFWMzY4LjRINTMuN1Y0MzRIMzlMMTYuNywzOTkuNFY0MzRIMFoiIHN0eWxlPSJmaWxsOiM0ODQyNDIiLz48cGF0aCBkPSJNODguNiw0MTEuOSw2Ni4zLDM2OC40SDg0bDEzLjEsMjYuM2MyLjktNS44LDYuNy0xNCwxMy4yLTI2LjNoMTcuM2wtMjIuMiw0My41VjQzNEg4OC42WiIgc3R5bGU9ImZpbGw6IzQ4NDI0MiIvPjxwYXRoIGQ9Ik0xMzguMywzNjguM2gyNC44YzE5LjUsMCwyNC4yLDEwLjksMjQuMiwxOS43YTE3LjU0NiwxNy41NDYsMCwwLDEtNC4xLDExLjIsMTguMywxOC4zLDAsMCwxLDguNCwxNC44YzAsOC45LTMuOCwxOS45LTI4LjQsMTkuOUgxMzguNFYzNjguM1ptMTYuOSwyNmg2LjljNy4zLDAsOC42LTMuOCw4LjYtNi4ycy0uNi02LjUtNy41LTYuNWgtOFptMCwyNi41aDcuOWMxMC4zLS4yLDExLjctMy4xLDExLjctNi43cy0yLjMtNi44LTEyLjctNi44aC02LjlaIiBzdHlsZT0iZmlsbDojNDg0MjQyIi8+PHBhdGggZD0iTTIwNC4yLDM2OC4zSDIyOWMxOS41LDAsMjQuMiwxMC45LDI0LjIsMTkuN2ExNy41NDYsMTcuNTQ2LDAsMCwxLTQuMSwxMS4yLDE4LjMsMTguMywwLDAsMSw4LjQsMTQuOGMwLDguOS0zLjgsMTkuOS0yOC40LDE5LjlIMjA0LjNWMzY4LjNabTE2LjgsMjZoNi45YzcuMywwLDguNi0zLjgsOC42LTYuMnMtLjYtNi41LTcuNS02LjVoLThabTAsMjYuNWg3LjljMTAuMy0uMiwxMS43LTMuMSwxMS43LTYuN3MtMi4zLTYuOC0xMi43LTYuOEgyMjFaIiBzdHlsZT0iZmlsbDojNDg0MjQyIi8+PHBhdGggZD0iTTI3MCwzNjguM2gxNi43djUyLjJoMjUuMVY0MzRIMjcwWiIgc3R5bGU9ImZpbGw6IzQ4NDI0MiIvPjxwYXRoIGQ9Ik0zMjUuNiwzNjguM2g0Mi41djEzLjJIMzQyLjR2MTMuN2gyMS41djEzLjJIMzQyLjR2MTIuM2gyNi40VjQzNEgzMjUuNlYzNjguM1oiIHN0eWxlPSJmaWxsOiM0ODQyNDIiLz48L3N2Zz4="
```

### Rôle Nybble
Utilisez la console de développement Elastic pour importer le rôle `nybble_analyst_hunter` :

```json
POST _security/role/nybble_analyst_hunter
{
    "cluster": [],
    "indices": [
      {
        "names": [
          "logs-*"
        ],
        "privileges": [
          "read",
          "read_cross_cluster",
          "view_index_metadata"
        ],
        "field_security": {
          "grant": [
            "*"
          ],
          "except": []
        },
        "allow_restricted_indices": false
      },
      {
        "names": [
          ".*-nybble"
        ],
        "privileges": [
          "write",
          "read",
          "view_index_metadata",
          "maintenance",
          "manage"
        ],
        "field_security": {
          "grant": [
            "*"
          ]
        },
        "allow_restricted_indices": false
      }
    ],
    "applications": [
      {
        "application": "kibana-.kibana",
        "privileges": [
          "feature_discover.all",
          "feature_dashboard.all",
          "feature_canvas.all",
          "feature_maps.all",
          "feature_graph.all",
          "feature_visualize.all"
        ],
        "resources": [
          "space:nybble"
        ]
      }
    ],
    "run_as": [],
    "metadata": {},
    "transient_metadata": {
      "enabled": true
    }
  }
```

### Nybble rolemapping
Utilisez la console de développement Elastic pour importer le mappage de rôle `nybble_analyst_hunter_role_mapping` :

```json
POST _security/role_mapping/nybble_analyst_hunter_role_mapping
{
    "enabled": true,
    "roles": [
      "nybble_analyst_hunter"
    ],
    "rules": {
      "all": [
        {
          "field": {
            "realm.name": "nybble"
          }
        },
        {
          "field": {
            "groups": "*"
          }
        }
      ]
    },
    "metadata": {}
  }
```

### Dataview
Pour que les analystes puissent voir les données présentes dans les index autorisés via le rôle, il faut créer une dataview.  
Les droits et le space ci-dessus sont trop restrictifs pour le faire, donc il faut créer cette dataview dans un autre space puis la transférer dans le space Nybble.