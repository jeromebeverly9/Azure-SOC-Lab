# Azure-SOC-Lab
# Building a SOC + Honeynet in Azure (Live Traffic)
<a href="https://imgur.com/2nX1Hxh"><img src="https://i.imgur.com/2nX1Hxh.png" title="source: imgur.com" /></a>

## Introduction

In this project, I set up a mini honeynet in Azure and collected log data from various sources and ingested them into a Log Analytics workspace. Microsoft Sentinel utilized this data to create attack maps, trigger alerts, and generate incidents. I measured security metrics in the unsecured environment over a 24-hour period, implemented security controls to strengthen the environment, then measured the metrics again for another 24 hours. The results are presented below. 

The metrics shown in this lab are: <br>

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
<a href="https://imgur.com/1CpSCfS"><img src="https://i.imgur.com/1CpSCfS.png" title="source: imgur.com" /></a>

## Architecture After Hardening / Security Controls
<a href="https://imgur.com/dUsUb1f"><img src="https://i.imgur.com/dUsUb1f.png" title="source: imgur.com" /></a>

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, and openly exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls WIDE OPEN, and all other resources are deployed with public endpoints visible to the Internet (no Private Endpoints).

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoints.

## Attack Maps BEFORE Hardening / Security Controls
- Windows RDP Auth Fail 
![windows-rdp-auth-fail](https://imgur.com/2k2GjOw.png)<br>
- Linux Syslog Auth Failures
<a href="https://imgur.com/3x6VSYA"><img src="https://i.imgur.com/3x6VSYA.png" title="source: imgur.com" /></a>
- NSG-Malicious Allowed In
<a href="https://imgur.com/Q1NVqJF"><img src="https://i.imgur.com/Q1NVqJF.png" title="source: imgur.com" /></a>
- MsSql-auth-fail
<a href="https://imgur.com/HatcM02"><img src="https://i.imgur.com/HatcM02.png" title="source: imgur.com" /></a>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:

Start Time 6/13/2024 12:34:10 <br>

Stop Time 6/14/2024 12:34:10 <br>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 18309
| Syslog                   | 59370
| SecurityAlert            | 8
| SecurityIncident         | 297
| AzureNetworkAnalytics_CL | 1563

## Attack Maps AFTER Hardening / Security Controls

```All map queries, except for the Linux failed-Auth logs, actually returned no results due to no instances of malicious activity for the 24 hour period after hardening. Even though the Linux VM was set to only record AUTH logs, the volume of recorded 'failed authorizations' is drastically lower compared to when the Linux VM was unprotected and facing the public internet.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:

Start Time 6/25/2024 14:44:09 <br>

Stop Time	 6/26/2024 14:44:09 <br>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10504
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was built in Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was then used to trigger alerts and create incidents based on the ingested logs. Metrics were measured in the insecure environment before applying security controls and then again after implementing the security measures. 

Notably, the number of security events and incidents significantly decreased after the security controls were applied, demonstrating their effectiveness.
