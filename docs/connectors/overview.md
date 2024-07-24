<center>![Nybble Logo](../img/Nybble-Logo-Full-Text2-005F93.svg){: style="width:800px;"}</center>

# Bring your Own SIEM/EDR -> Connectors
You have a SIEM (or an EDR) which centralizes all your logs, trigger some alerts, but no processing is done ?  
You can connect your solution to Nybble Hub and let our community do the job for you.

## How does it work ?
Your solution is sending alert payloads to Nybble Hub. A connector must be declared on Hub side, then configured as a target in your solution (usually with webhooks).  
Analysts from Nybble requires an access to your detection solution, to do investigations. We're providing you all the details in dedicated pages per vendor.

!!! note
    Starting at release 2024.06.01, the access to your detection solution (using SSO) can be optional. However, it drastically restrict analysis and decision capabilities of our Nybblers.
    We suggest avoiding it unless you're on a restricted environment, for a specific perimeter only.