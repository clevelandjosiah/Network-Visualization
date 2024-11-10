# Network-Visualization
# Analyzing Netwokr Traffic Using Wireshark and Google Maps
![image](https://github.com/clevelandjosiah/Azure-Honeynet/assets/171377691/ccbb0cae-4da6-4481-aa67-93363a41dd67)


## Introduction

In this project, I created a mini honeynet in Azure and collected log data from various sources into a Log Analytics workspace. This workspace was then utilized by Microsoft Sentinel to develop attack maps, trigger alerts, and generate incidents. I measured specific security metrics in the unsecured environment for 24 hours, implemented security controls to enhance the environment's security, and then measured the metrics again for another 24 hours. The metrics I collected are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![image](https://github.com/clevelandjosiah/Azure-Honeynet/assets/171377691/db74ab11-6f48-411c-9a6b-50da5ff22be8)


## Architecture After Hardening / Security Controls
![image](https://github.com/clevelandjosiah/Azure-Honeynet/assets/171377691/173f8d5a-7852-4a3a-b261-8deea35f808c)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

Additionally, the SOC utilized the following tools, components and regulations:

- Microsoft Sentinel (SIEM)
- Microsoft Defender for Cloud (MDC)
- NIST SP 800-53 Revision 4
- PCI DSS 3.2.1
- Log Analytics Workspace
- Windows Event Viewer
- Kusto Query Language (KQL)
- PowerShell


For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, I set up the built-in firewalls on the virtual machines to restrict access and safeguard the resources against unauthorized connections. I strengthened the NSGs by blocking all incoming and outgoing traffic, except for traffic coming from my own public IP address. I swapped the public endpoints with Private Endpoints. This made sure that sensitive resources, like databases and storage accounts, could only be accessed from within the virtual network and not from the public internet.


## Attack Maps Before Hardening / Security Controls

NSG Allowed Malicious Inbound Flows
<img width="1266" alt="(before) nsg-malicious-allowed-in" src="https://github.com/clevelandjosiah/Azure-Honeynet/assets/171377691/c1765122-eec6-4859-b70e-429b711ef951">

Linux SSH Authentication Failures 
<img width="1266" alt="(before) linux-ssh-auth-fail" src="https://github.com/clevelandjosiah/Azure-Honeynet/assets/171377691/e6891142-b587-4fbc-920e-490807fd4556">

Windows RDP/SMB Authentication Failures 
<img width="1266" alt="(before) windows-rdp-auth-fail" src="https://github.com/clevelandjosiah/Azure-Honeynet/assets/171377691/36ad413c-c7bc-4080-b3fb-6551a8ae9c24">

MS SQL Server Authentication Failures 
<img width="1266" alt="(before) mssql-auth-fail" src="https://github.com/clevelandjosiah/Azure-Honeynet/assets/171377691/1c23832c-508b-4808-8996-e2ed9061a28f">



## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-05-14 13:35:32
Stop Time 2024-05-15 13:35:32

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 53531
| Syslog                   | 367
| SecurityAlert            | 6
| SecurityIncident         | 245
| AzureNetworkAnalytics_CL | 2061

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## NSG Allowed Malicious Inbound Flows
![image](https://github.com/clevelandjosiah/Azure-Honeynet/assets/171377691/51093068-04a3-4655-8418-1eebfffdc867)


## Linux SSH Authentication Failures
![image](https://github.com/clevelandjosiah/Azure-Honeynet/assets/171377691/20f1ff73-581a-4110-ac78-29b7c194f8b9)


## Windows RDP/SMB Authentication Failures
![image](https://github.com/clevelandjosiah/Azure-Honeynet/assets/171377691/112c0caa-5e55-460e-96a1-6c0e9d51e52b)


## MS SQL Server Authentication Failures
![image](https://github.com/clevelandjosiah/Azure-Honeynet/assets/171377691/63df73f5-05e3-45c8-a494-3ce580c7eee4)


## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-05-30 18:25:02
Stop Time	2024-05-31 18:25:02

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Impact of Security Controls

| Metric                   | Change post-hardening 
| ------------------------ | -----
| SecurityEvent (Windows VMs) | 83.73%
| Syslog (Linux VMs) | 99.18%
| SecurityAlert (Microsoft Defender for Cloud) | 100%
| SecurityIncident (Sentinel Incidents) | 100%
| AzureNetworkAnalytics_CL | 100%



## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
