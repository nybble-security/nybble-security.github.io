# Elastic Connector How to
!!! note 
    This documentation suits only for Bring Your Own SIEM (BYOS) customers.  
    If you are an Elastic By Nybble customer, all configurations are already done.

## Overview
By following this page, you will setup the connection between your SIEM and Nybble Hub.  
It consists of an endpoint on Hub side, and a webhook connector on Elastic side.

## Hub side: connector
1. Connect to Nybble Hub using your usual credentials
2. Go to Settings > Connectors
3. Add an Elastic connector then fill the form:

    | Field | Explanation | Usual value |
    | ------ | ----------- | -------- |
    | Display Name | name to display during authentication and in hub configs | `elastic` |
    | Kibana URL | root URL of Kibana, will be used to forge all URLs to access your SIEM from Hub | `https://contoso.kb.northeurope.azure.elastic-cloud.com:9243` |

4. Click on Save

!!! warning
    At this stage, connector password will be generated and available in a popup.  
    Be sure to copy and store this password in a secure location as it will not be displayed anymore !  
    You can always reset it afterwards, but you will have to update any webhooks with the new value.

## Elastic side: webhook connector
!!! note
    This step requires admin rights on Elastic side

1. Go to Stack Management -> Alerts and Insights -> Connectors and click on `Create Connector`
2. Select the `Webhook` type
3. Fill the fields:

    | Field | Explanation | Usual value |
    | ------ | ----------- | -------- |
    | Connector Name | display name | `nybble` |
    | Connector Settings / Method | - | POST |
    | Connector Settings / URL | URL of central nybble connectors endpoint | `https://connectors.nybble-analytics.io/conn/elastic` |
    | Authentication / Username,Password | connector authentication | values from `Hub side: connector` |
    | Authentication / HTTP Header | additional infos, required | key: `Content-Type` <br> Value: `text/plain` |

4. Click on save.

The final steps will be to use this connector on any security rule actions, in order to send raised alerts to Nybble services.  
Usually this step is done with Nybble Professional Services according to detection perimeter.