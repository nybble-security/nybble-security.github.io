# Nybble Security Analytics: Onboard a Windows
Winlogbeat is the agent to install on a Windows to onboard it.

## Before you begin
Connect to [Nybble Hub](https://hub.nybble-analytics.io/settings/agents){target=_blank} at the Settings section to:

* download the Winlogbeat package: it contains software + configuration files
* grab your client short name: it will be used in network rules.

## Winlogbeat installation

Winlogbeat itself has no sizing requirements: it's a lightweight agent.

1. Extract the contents of winlogbeat-package into C:\Program Files.
2. Rename the winlogbeat-<version> directory to Winlogbeat.
3. Open a PowerShell prompt as an Administrator (right-click on the PowerShell icon and select Run As Administrator).
4. From the PowerShell prompt, run the following commands to install the service.
```
PS C:\Users\Administrator> cd 'C:\Program Files\Winlogbeat'
PS C:\Program Files\Winlogbeat> .\install-service-winlogbeat.ps1

Security warning
Run only scripts that you trust. While scripts from the internet can be useful,
this script can potentially harm your computer. If you trust this script, use
the Unblock-File cmdlet to allow the script to run without this warning message.
Do you want to run C:\Program Files\Winlogbeat\install-service-winlogbeat.ps1?
[D] Do not run  [R] Run once  [S] Suspend  [?] Help (default is "D"): R

Status   Name               DisplayName
------   ----               -----------
Stopped  winlogbeat         winlogbeat
```
!!! note 
    If script execution is disabled on your system, you need to set the execution policy for the current session to allow the script to run.  
    Example: `powershell.exe -ExecutionPolicy unrestricted -file .\install-service-winlogbeat.ps1`

## Network rules

Winlogbeat agent requires following network rule to communicate with Nybble's servers:

| Source | Destination | Protocol | Usage | 
| ------ | ----------- | -------- | ----- |
| winlogbeat server | `<clientshortname>-kafka-bootstrap.nybble-analytics.io` <br> `<clientshortname>-kafka-broker-[0-9].nybble-analytics.io` | TCP 9094 | Event sending |