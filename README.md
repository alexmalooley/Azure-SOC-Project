# Building a SOC + Honeynet in Azure (Live Traffic)

![Cloud Honeynet / SOC](https://i.imgur.com/TKOJI1E.png)

## Introduction

In this project, I constructed a small-scale honeynet within Azure, collecting log data from diverse sources into a Log Analytics workspace. This workspace serves as the foundation for Microsoft Sentinel to construct attack visualizations, generate alerts, and manage security incidents. I evaluated security metrics within the vulnerable environment over a 48-hour period, implemented security measures to fortify the environment, conducted another 48-hour metric assessment, and present the findings below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- NSG Inbound Malicious Flows Allowed (Malicious Flows allowed into our honeynet)

## Disclaimer

I let the environment run insecure from 4/26 to 4/28 to collect data. Then, due to a trip, I had to turn off my VMs as I didn't have time to manage them. Upon returning from my trip, I turned on the VMs, conducted an incident response, hardened the environment, and left it secure for another 48 hours from 5/12 to 5/14.

## Architecture Before Hardening / Security Controls

![Architecture Diagram Before](https://i.imgur.com/1Em0R4C.jpeg)

## Architecture After Hardening / Security Controls

![Architecture Diagram After](https://i.imgur.com/kPhI7VA.jpeg)

To build this mini honeynet in Azure, I used several key elements, including:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

Initially, all resources were openly accessible on the internet. Virtual Machines had their Network Security Groups and internal firewalls configured with broad accessibility, while other resources were deployed with public endpoints, making Private Endpoints unnecessary.

Post-hardening, the security posture was significantly tightened. Network Security Groups were configured to block all traffic except from my administrative workstation, and other resources were protected by their internal firewalls and the implementation of Private Endpoints.

## Attack Maps Before Hardening / Security Controls

![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/biaiP0h.png)
![Linux Syslog Auth Failures](https://i.imgur.com/RNSrwxg.png)
![Windows RDP/SMB Auth Failures](https://i.imgur.com/0MwmDTS.png)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 48 hours:
Start Time 2024-04-26 17:04:29
Stop Time 2024-04-28 17:04:29

| Metric | Count |
| ------- | ----- |
| SecurityEvent | 147494 |
| Syslog | 3544 |
| SecurityAlert (Microsoft Defender for Cloud) | 6 |
| SecurityIncident (sentinel) | 329 |
| NSG Inbound Malicious Flows Allowed | 2465 |

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 48 hours, after applying security controls:
Start Time 2024-05-12T15:55:55
Stop Time 2024-05-14 15:55

| Metric | Count |
| ------- | ----- |
| SecurityEvent | 9016 |
| Syslog | 1 |
| SecurityAlert | 0 |
| SecurityIncident | 0 |
| NSG Inbound Malicious Flows Allowed | 0 |

## Improvement After Hardening

Following the hardening measures, significant improvements in security metrics were observed:

- **Security Events (Windows VMs):** Reduction of 93.89%
- **Syslog (Linux VMs):** Reduction of 99.97%
- **SecurityAlert (Microsoft Defender for Cloud):** Reduction of 100.00%
- **Security Incident (Sentinel Incidents):** Reduction of 100.00%
- **NSG Inbound Malicious Flows Allowed:** Reduction of 100.00%

`All map queries actually returned no results due to no instances of malicious activity for the 48-hour period after hardening.`

## Conclusion

This project set up a compact honeynet in Microsoft Azure, integrating log sources into a Log Analytics workspace and utilizing Microsoft Sentinel to generate alerts and manage incidents. Security metrics were assessed both before and after implementing security measures. The post-implementation results show a significant decrease in security events and incidents, demonstrating the efficacy of the security controls.

It is important to acknowledge that if the network's resources were extensively used by regular users, more security events and alerts might have been generated within the 48-hour period following the deployment of the security controls.


