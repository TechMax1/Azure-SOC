# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud SOC (1)](https://github.com/TechMax1/Azure-SOC/assets/155503899/6dd183e3-0f5e-412f-9d15-629ae67de7a9)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Security Controls Before Hardening](https://github.com/TechMax1/Azure-SOC/assets/155503899/70052bd1-dfc2-49fe-b8f3-91d188ba38a6)

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
![before-windows_rdp_auth_fail](https://github.com/TechMax1/Azure-SOC/assets/155503899/6799c0c4-b36f-4931-ae6d-1fa516e9ec7d)
![before-linux_24hrs](https://github.com/TechMax1/Azure-SOC/assets/155503899/8d6a1b69-d3a5-4343-984a-a7314342e4c0)
![before-sql_server_24hrs](https://github.com/TechMax1/Azure-SOC/assets/155503899/920d933a-2917-4a11-8e36-c56db6fb47ac)
![before-nsg_malicious_allowed_in](https://github.com/TechMax1/Azure-SOC/assets/155503899/02b09ab8-19d9-4b0d-b62b-d6d128cd0e06)
## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-03-06T15:17:23
Stop Time  2024-03-07T15:17:23

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 66497
| Syslog                   | 2672
| SecurityAlert            | 4
| SecurityIncident         | 201
| AzureNetworkAnalytics_CL | 2423

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
