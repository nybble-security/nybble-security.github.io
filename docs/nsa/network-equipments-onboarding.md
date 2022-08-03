# Nybble Security Analytics: Onboard a Network Device

## Before you begin
Onboarding a network device requires a filebeat collector somewhere in your network.  
See [Nybble Security Analytics: Filebeat collector](filebeat-collector.md) for further details.

Grab the DNS name of the nearest filebeat collector you've installed in your network.

## Cisco IOS onboarding
Log collection will use syslog, on port 1820, UDP.  
Please refer to Cisco IOS documentation related to your device version. Usually, commands to enable syslog are:  
```
logging host <filebeat_ip>
logging trap informational
```  

