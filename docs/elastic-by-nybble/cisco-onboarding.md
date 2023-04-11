# Nybble Security Analytics: Onboard a Cisco Device

## Before you begin
Onboarding a network device requires a filebeat collector somewhere in your network.  
See [Nybble Security Analytics: Filebeat collector](filebeat-collector.md) for further details.

Grab the DNS name of the nearest filebeat collector you've installed in your network.

## Cisco AMP onboarding

To configure the Cisco AMP fileset you will need to retrieve your `client_id` and `api_key` from the AMP dashboard.  
For more information on how to retrieve these credentials, please reference the [Cisco AMP API documentation](https://api-docs.amp.cisco.com/api_resources?api_host=api.amp.cisco.com&api_version=v1).

The URL configured for the API depends on which region your AMP is located, currently there are three choices:  

- api.amp.cisco.com
- api.apjc.amp.cisco.com
- api.eu.amp.cisco.com

If new endpoints are added by Cisco in the future, please reference the API URL list located at the [Cisco AMP API Docs](https://api-docs.amp.cisco.com/).

To configure the collection on filebeat:  

1. Locate the cisco AMP section in filebeat.yml to turn on the AMP collection:
```yaml
  amp:
    enabled: false # turn it to true
```
2. On the same section, fill the `client_id` and `api_key` values:
```yaml
    var.client_id: <client_id>
    var.api_key: <api_key>
```
3. Restart the filebeat agent.

Source for more details: [Elastic Filebeat : AMP module](https://www.elastic.co/guide/en/beats/filebeat/7.17/filebeat-module-cisco.html#_amp_fileset_settings)

## Cisco IOS onboarding
1. Locate the cisco IOS section in filebeat.yml to turn on the IOS collection:
```yaml
  ios:
    enabled: false # turn it to true
```
Log collection will use syslog, on port 1820, UDP. 
2. restart the filebeat agent. 
3. Please refer to Cisco IOS documentation related to your device version. Usually, commands to enable syslog are:  
```
logging host <filebeat_ip>
logging trap informational
```  

Source for more details: [Elastic Filebeat : IOS module](https://www.elastic.co/guide/en/beats/filebeat/7.17/filebeat-module-cisco.html#_ios_fileset_settings)

## Cisco Umbrella onboarding
The Cisco Umbrella fileset primarily focuses on reading CSV files from an S3 bucket using the filebeat S3 input.

To configure Cisco Umbrella to log to a self-managed S3 bucket please follow the [Cisco Umbrella User Guide](https://docs.umbrella.com/deployment-umbrella/docs/log-management), and the [AWS S3 input documentation](https://www.elastic.co/guide/en/beats/filebeat/7.17/filebeat-input-aws-s3.html) to setup the necessary Amazon SQS queue.

Nybble has enabled the 4 supported filesets collection:  

- Proxy
- Cloud Firewall 
- IP Logs 
- DNS logs

To configure the collection on filebeat:  

1. Locate the cisco Umbrella section in filebeat.yml to turn on the Umbrella collection:
```yaml
  umbrella:
    enabled: false # turn it to true
```
2. On the same section, fill the `queue_url`, `access_key_id` and `secret_access_key` values:
```yaml
    var.queue_url: https://<AWS S3 SQS Queue URL>
    var.access_key_id: <access_key_id>
    var.secret_access_key: <secret_access_key>
```
3. Restart the filebeat agent.

Source for more details: [Elastic Filebeat : Umbrella module](https://www.elastic.co/guide/en/beats/filebeat/7.17/filebeat-module-cisco.html#_umbrella_fileset_settings)