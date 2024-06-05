# Building a SOC + Honeynet in Azure (Live Traffic)
<img src="https://cdn.discordapp.com/attachments/929786441008021504/1116841350231490590/New_Project_10.png?ex=6661814f&is=66602fcf&hm=d5b75bb22d59220f6946e169413c21c6051957bf355d12cc7c423895f08c7f3e&">

## Introduction

In this project, I built a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
<img src="https://cdn.discordapp.com/attachments/929786441008021504/1116844379638857759/New_Project_11.png">  

## Architecture After Hardening / Security Controls
<img src="https://cdn.discordapp.com/attachments/929786441008021504/1116846996842627222/New_Project_12.png">  

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
<img src="https://cdn.discordapp.com/attachments/929786441008021504/1116852980369592330/Before_Windows_rdp_auth.png">
<img src="https://cdn.discordapp.com/attachments/929786441008021504/1116852980960985159/Before_Syslog_ssh_auth.png">
<img src="https://cdn.discordapp.com/attachments/929786441008021504/1116852981242019850/Before_Nsg_Malicious_Allowed.png">

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 71836
| Syslog                   | 1875
| SecurityAlert            | 2
| SecurityIncident         | 315
| AzureNetworkAnalytics_CL | 1151

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 11460
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.
