# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC] ![image](https://github.com/user-attachments/assets/195880a6-6bcc-4ab7-abef-32804b05c6cc)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram]![Before-Main Hardening](https://github.com/user-attachments/assets/e7deadbb-0842-4245-a06b-ad1f5b13e7a7)


## Architecture After Hardening / Security Controls
![012922_1955_Azurepolicy1 after Hardening](https://github.com/user-attachments/assets/a2e4668e-044a-4ec0-b052-df3d29669112)


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
![NSG Allowed Inbound Malicious Flows]![(before) nsg-malicious-allowed-in](https://github.com/user-attachments/assets/e715b262-217a-4be6-ad12-b0e15bd1777e)

![Linux Syslog Auth Failures]![(before) linux-ssh-auth-fail](https://github.com/user-attachments/assets/b9ba73ff-9d61-4029-92a5-11643777806f)

![Windows RDP/SMB Auth Failures]![(before) windows-rdp-auth-fail](https://github.com/user-attachments/assets/310d0a0c-fc6d-4029-9af5-590cf3c09bd6)

![MySQL Auth Failures]![(before) mssql-auth-fail](https://github.com/user-attachments/assets/54cfac2e-3bd2-44c7-8aae-29e40b34506a)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 8/21/2024 21:47:21
Stop Time 8/22/2024 21:47:21

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 38476
| Syslog                   | 2432
| SecurityAlert            | 4
| SecurityIncident         | 188
| AzureNetworkAnalytics_CL | 740

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 8/27/2024 23:03:38
Stop Time	8/28/2024 23:03:38

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 17989
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
