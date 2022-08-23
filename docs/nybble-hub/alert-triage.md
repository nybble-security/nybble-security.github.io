# Alert Triage
!!! note 
    An L1 analyst or L2 analyst profile is required to follow this documentation

## Alert main table
![Alert Main Table](../img/nybble-hub/alert-triage/alert-triage-main-table.png){: style="width:800px;"}

Alerts are displayed by Severity, then Created Date, oldest first.  
To process an alert, click on its name. Alert will be affected to you automatically.

!!! note 
    An alert is automatically unlocked after 1h of inactivity.

## Alert details and processing
![Alert Processing](../img/nybble-hub/alert-triage/alert-triage-processing.png){: style="width:800px;"}

### (1) Summary
Useful informations are:

 - processing Time: SIEM timestamp when the alert has been raised
 - occurrences: how many times the same alert (fields, SIGMA rule) have been raised in a 1h timeframe

### (2) Sigma
All useful informations extracted from the event(s), matched to the SIGMA rule:

  - tags: given by SIGMA
  - fields: values of main fields extracted. These fields are the root of the triage
  - references: given by SIGMA, useful external articles to get more informations on the alert

### (3) Raw Event(s)
On this panel you see a hierarchical representation of the original event as it is on the SIEM before matching.  
Useful to grab more informations than displayed using the fields.

### (4) Actions
Several actions are available:

  - View Logs: opens the according SIEM and display the event in the discovery panel.  
  You can do more request to find additional informations or context in order to make your decision on the alert triage
  - Wiki: specific, dedicated Wiki page (per SIGMA rule) to help you on the alert triage. It opens on a side panel for convenience.
  - Release / Unlock the alert: not ready to finish the triage? Release the alert to let another analyst looking at it.

### (5) Decision
Final decision about the current alert:

  - no incident: Nybble will adjust the rule to avoid further alerts
  - incident: Nybble L2 team will investigate
  - escalate: you're not able to take the decision, or it requires more investigation, so you escalate to L2 team.

