# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this endeavor, I constructed a small-scale honeynet within the Azure cloud platform. The honeynet was designed to gather log data from multiple sources and store it in a Log Analytics workspace. Microsoft Sentinel was utilized to analyze the data, generating attack maps, activating alerts, and creating incident reports. The project involved assessing security metrics in an unsecured environment over a 24-hour period. Subsequently, security controls were implemented to fortify the environment, after which the security metrics were measured again for another 24 hours. The following section presents the obtained results for the metrics we focused on.

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
<b>Windows RDP/SMB Auth Failures</b>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/W8Ka3N1.jpg)<br>
<b>Linux Syslog Auth Failures</b>
![Linux Syslog Auth Failures](https://i.imgur.com/M29LpCp.jpg)<br>
<b>NSG Allowed Inbound Malicious Flows</b>
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/zjd23xz.jpg)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:<br>
Start Time: 2023-07-17 02:11:43<br>
Stop Time: 2023-07-18 02:11:43

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 30625
| Syslog                   | 1540
| SecurityAlert            | 1
| SecurityIncident         | 225
| AzureNetworkAnalytics_CL | 1897

## Attack Maps After Hardening / Security Controls

All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:<br>
Start Time: 2023-07-19 02:08:20<br>
Stop Time:	2023-07-20 02:08:20

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8729
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

<b>Percentage Decrease After Securing/Hardening Envionment</b><br>
SecurityEvent: -71.5%<br>
Syslog: -98.38%<br>
SecurityAlert: -100%<br>
SecurityIncident: -100%<br>
NSG Inbound Malicious Flows Allowed: -100%

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
