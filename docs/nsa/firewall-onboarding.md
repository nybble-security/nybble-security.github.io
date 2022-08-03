# Nybble Security Analytics: Onboard a Firewall

## Before you begin
Onboarding a firewall device requires a filebeat collector somewhere in your network.  
See [Nybble Security Analytics: Filebeat collector](filebeat-collector.md) for further details.

Grab the DNS name of the nearest filebeat collector you've installed in your network.

## Fortigate onboarding
Log collection will use syslog, on port 1801, UDP.  
Refer to [Fortinet CLI Documentation](https://docs.fortinet.com/document/fortigate/6.0.0/cli-reference/260508/log-syslogd-syslogd2-syslogd3-syslogd4-setting){target=_blank} for device configuration.  
The syslog format choosen should be `Default`.