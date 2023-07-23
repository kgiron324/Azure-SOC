# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I constructed a small-scale honeynet within the Azure cloud platform. The honeynet was designed to gather log data from multiple sources and store it in a Log Analytics workspace. Microsoft Sentinel was utilized to analyze the data, generating attack maps, activating alerts, and creating incident reports. The project involved assessing security metrics in an unsecured environment over a 24-hour period. Subsequently, security controls were implemented to fortify the environment, after which the security metrics were measured again for another 24 hours. The following section presents the obtained results for the metrics we focused on.

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the small honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

Regarding the "BEFORE" metrics, all resources were initially deployed and made accessible to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls configured with unrestricted access, while other resources were deployed with public endpoints visible on the Internet. Consequently, there was no utilization of Private Endpoints during this phase.

Regarding the "AFTER" metrics, significant security enhancements were implemented. Network Security Groups were strengthened by blocking ALL traffic, except for my admin workstation, which was allowed to access the resources. Additionally, all other resources benefited from increased protection through their built-in firewalls and the use of Private Endpoints.

## Attack Maps Before Hardening / Security Controls
<b>Windows RDP/SMB Auth Failures</b>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/W8Ka3N1.jpg)<br>
<b>Linux Syslog Auth Failures</b>
![Linux Syslog Auth Failures](https://i.imgur.com/M29LpCp.jpg)<br>
<b>NSG Allowed Inbound Malicious Flows</b>
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/zjd23xz.jpg)<br>

## Metrics Before Hardening / Security Controls

The following table displays the metrics I recorded within the unsecured environment over a 24-hour period:<br>
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

After hardening the environment and implenting security controls, all map queries yielded no results as there were no instances of malicious activity detected during the 24-hour period.

## Metrics After Hardening / Security Controls


The following table displays the metrics I assessed within the environment for an additional 24-hour period, after the implementation of security controls:<br>
Start Time: 2023-07-19 02:08:20<br>
Stop Time:	2023-07-20 02:08:20

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8729
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

<b>Decrease in Malicious Activity (%) After Securing Environment</b><br>
SecurityEvent: -71.5%<br>
Syslog: -98.38%<br>
SecurityAlert: -100%<br>
SecurityIncident: -100%<br>
NSG Inbound Malicious Flows Allowed: -100%

## Conclusion

This project involved building a small honeynet in Microsoft Azure and integrating log sources into a Log Analytics workspace. Microsoft Sentinel was utilized to trigger alerts and create incidents based on the collected logs. To evaluate the impact of security controls, metrics were measured in the insecure environment prior to applying security measures and then again after implementing them. The results revealed a significant reduction in the number of security events and incidents, indicating the effectiveness of the security controls.

It should be noted that if the network's resources were heavily utilized by regular users, it is possible that more security events and alerts might have been generated within the 24-hour period following the implementation of the security controls.
