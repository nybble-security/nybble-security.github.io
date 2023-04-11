# Nybble Security Analytics: Onboard a Linux
Auditbeat is the agent to install on a Linux to onboard it.

## Before you begin
Connect to [Nybble Hub](https://hub.nybble-analytics.io/settings/agents){target=_blank} at the Settings section to:

* download the auditbeat configuration files
* grab your client short name: it will be used in network rules.

## Auditbeat installation

Auditbeat itself has no sizing requirements: it's a lightweight agent (less than 2% CPU).

To download and install Auditbeat, use the commands that work with your system:

=== "Debian"

    ```
    curl -L -O https://artifacts.elastic.co/downloads/beats/auditbeat/auditbeat-8.3.3-amd64.deb
    sudo dpkg -i auditbeat-8.3.3-amd64.deb
    ```

=== "CentOS"

    ```
    curl -L -O https://artifacts.elastic.co/downloads/beats/auditbeat/auditbeat-8.3.3-x86_64.rpm
    sudo rpm -vi auditbeat-8.3.3-x86_64.rpm
    ```

## Auditbeat configuration

The auditbeat-config.zip file contains all the required files to run auditbeat:  

* yml configuration files
* certs subfolder : certificates to secure communication to Nybble's servers

Copy these contents into the auditbeat agent folder :

=== "Debian"

    ```
    /etc/auditbeat
    ```

=== "CentOS"

    ```
    /etc/auditbeat
    ```

Then start the program :

=== "Debian"

    ```
    sudo systemctl enable auditbeat.service    # enable at boot
    sudo service auditbeat start
    ```

=== "CentOS"

    ```
    sudo systemctl enable auditbeat.service    # enable at boot
    sudo service auditbeat start
    ```

## Network rules

Auditbeat agent requires following network rule to communicate with Nybble's servers:

| Source | Destination | Protocol | Usage | 
| ------ | ----------- | -------- | ----- |
| auditbeat server | `<clientshortname>-kafka-bootstrap.nybble-analytics.io` <br> `<clientshortname>-kafka-broker-[0-9].nybble-analytics.io` | TCP 9094 | Event sending |
