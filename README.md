# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

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
![NSG Allowed Inbound Malicious Flows]![(before)-nsg-malicious-allowed-in](https://github.com/rsaldana7285/Cloud-SOC/assets/141673315/21c209c1-97ac-4e77-a05d-cc2cb90b94e4)
![Linux Syslog Auth Failures]![(before)-syslog-ssh-auth-fail](https://github.com/rsaldana7285/Cloud-SOC/assets/141673315/ae3db307-ba8d-4961-98f8-914f88a4aeea)
![Windows RDP/SMB Auth Failures]![(before)-windows-rdp-smb-auth-fail](https://github.com/rsaldana7285/Cloud-SOC/assets/141673315/3ce2b100-ef8a-44d5-a0d4-1c32233aeae5)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-08-01 14:24:55
Stop Time 2023-08-02 14:24:55

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 135924
| Syslog                   | 12133
| SecurityAlert            | 10
| SecurityIncident         | 427
| AzureNetworkAnalytics_CL | 10344

## Attack Maps Before Hardening / Security Controls

```All map queries returned no results of malicious activity for the 24 hour period after hardening. NIST 800-53 Regulatory Compliance Policy. Focusing on SC-7 enabling MFA and implementing Azure Firewall.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, after we have applied security controls:
Start Time 2023-08-04 20:51:32
Stop Time	2023-08-05 20:51:32

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 12861
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
