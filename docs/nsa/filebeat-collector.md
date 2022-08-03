# Nybble Security Analytics: Filebeat collector
Filebeat aims to act as a central event collector for your firewalls, network equipments ...

## Before you begin
Connect to [Nybble Hub](https://hub.nybble-analytics.io/settings/agents){target=_blank} at the Settings section to:

* download the Filebeat configuration files
* grab your client short name: it will be used in network rules.

## Sizing

Filebeat itself has no sizing requirements: it's a lightweight agent (less than 2% CPU).  
Nybble recommends using a dedicated VM / Server to run it, as there will be incoming / outgoing network rules to create later.  
System specifications can remain the usual ones in your company.  

## Filebeat installation

To download and install Filebeat, use the commands that work with your operating system ([compatibility matrix](https://www.elastic.co/fr/support/matrix){target=_blank}):

=== "Debian"

    ```
    curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.3.2-amd64.deb
    sudo dpkg -i filebeat-8.3.2-amd64.deb
    ```

=== "CentOS"

    ```
    curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.3.2-x86_64.rpm
    sudo rpm -vi filebeat-8.3.2-x86_64.rpm
    ```

=== "Windows"

    1. Download the Filebeat Windows zip file from the downloads page.
    2. Extract the contents of the zip file into C:\Program Files.
    3. Rename the filebeat-<version>-windows directory to Filebeat.
    4. Open a PowerShell prompt as an Administrator (right-click the PowerShell icon and select Run As Administrator).
    5. From the PowerShell prompt, run the following commands to install Filebeat as a Windows service:
    ```
    PS > cd 'C:\Program Files\Filebeat'
    PS C:\Program Files\Filebeat> .\install-service-filebeat.ps1
    ```
    !!! note 
        If script execution is disabled on your system, you need to set the execution policy for the current session to allow the script to run.  
        Example: `powershell.exe -ExecutionPolicy unrestricted -file .\install-service-filebeat.ps1.`

## Filebeat configuration

The filebeat-config.zip file contains all the required files to run filebeat:  

* yml configuration files
* certs subfolder : certificates to secure communication to Nybble's servers

Copy these contents into the filebeat agent folder :

=== "Debian"

    ```
    /etc/filebeat
    ```

=== "CentOS"

    ```
    /etc/filebeat
    ```

=== "Windows"

    ```
    C:\Program Files\Filebeat
    ```

Then start the program :

=== "Debian"

    ```
    sudo systemctl enable filebeat.service    # enable at boot
    sudo service filebeat start
    ```

=== "CentOS"

    ```
    sudo systemctl enable filebeat.service    # enable at boot
    sudo service filebeat start
    ```

=== "Windows"

    ```
    PS C:\Program Files\filebeat> Start-Service filebeat
    ```

    By default, Windows log files are stored in `C:\ProgramData\filebeat\Logs.`

## Network rules
Filebeat agent requires following network rule to communicate with Nybble's servers:

| Source | Destination | Protocol | Usage | 
| ------ | ----------- | -------- | ----- |
| filebeat server | `<clientshortname>-kafka-bootstrap.nybble-analytics.io` <br> `<clientshortname>-kafka-broker-[0-9].nybble-analytics.io` | TCP 9094 | Event sending |

## What's next
You can start to onboard [firewalls](firewall-onboarding.md) or [network devices](network-equipments-onboarding.md).
