# Azure-SOC-Lab
# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://imgur.com/2nX1Hxh.jpg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://imgur.com/1CpSCfS.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://imgur.com/dUsUb1f.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls WIDE OPEN, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps BEFORE Hardening / Security Controls
- Windows RDP Auth Fail 
![windows-rdp-auth-fail](https://imgur.com/2k2GjOw.png)<br>
- Linux Syslog Auth Failures
![Linux Syslog Auth Failures](https://imgur.com/3x6VSYA.png)<br>
- NSG-Malicious Allowed In
![NSG-Malicious Allowed In](https://imgur.com/Q1NVqJF.png)<br>
- MsSql-auth-fail
![MsSql-auth-fail](https://imgur.com/HatcM02.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 6/13/2024 12:34:10
Stop Time 6/14/2024 12:34:10

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 18309
| Syslog                   | 59370
| SecurityAlert            | 8
| SecurityIncident         | 297
| AzureNetworkAnalytics_CL | 1563

## Attack Maps AFTER Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 6/25/2024 14:44:09
Stop Time	6/26/2024 14:44:09

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10504
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
