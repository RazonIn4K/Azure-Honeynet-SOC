# Azure-Honeynet-SOC and SOC with Real-World Cyber Attacks

![Azure_screenshots](https://github.com/user-attachments/assets/4a7ac6cc-9351-4b43-ac3f-b86bfaee1b39)

Introduction and Objective

In this project, a mini honeynet was constructed within the Azure platform. The objective was to capture and analyze logs from several sources, subsequently consolidated within a Log Analytics workspace. Microsoft Sentinel was deployed to leverage these logs by developing attack maps, creating alert triggers, and incident generation. Azure Sentinel measured the metrics of an insecure environment over a 24-hour period. Following this phase, security controls were implemented to fortify the virtual environment. Lastly, another 24-hour metric measurement phase was conducted and the results obtained from these endeavors are presented below. The metrics analyzed were:

    SecurityEvent (Windows Event Logs)
    Syslog (Linux Event Logs)
    SecurityAlert (Log Analytics Alerts Triggered)
    SecurityIncident (Incidents created by Sentinel)
    AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

Technologies, Azure Components, and Regulations Employed

    Azure Virtual Network (VNet)
    Azure Network Security Groups (NSG)
    Virtual Machines (2 Windows VMs, 1 Linux VM)
    Log Analytics Workspace with Kusto Query Language (KQL) Queries
    Azure Key Vault for Secure Secrets Management
    Azure Storage Account for Data Storage
    Microsoft Sentinel for Security Information and Event Management (SIEM)
    Microsoft Defender for Cloud to Protect Cloud Resources
    Windows Remote Desktop for Remote Access
    Command Line Interface (CLI) for System Management
    PowerShell for Automation and Configuration Management
    NIST SP 800-53 Revision 5 for Security Controls
    NIST SP 800-61 Revision 2 for Incident Handling Guidance

Architecture BEFORE Hardening and Implementing Security Controls
![Azure_2](https://github.com/user-attachments/assets/1411e203-fc83-48b1-a8dc-fda9ff4bd237)
During the "BEFORE" stage of the project, a virtual environment was deployed and exposed to the public for malicious actors to discover. The intent for this stage was to attract bad actors and observe their attack patterns. To achieve this, A Windows virtual machine hosting a SQL database was deployed and a Linux server both had their network security groups (NSGs) had Allow All configured. To entice these attackers even further, a storage account and key vault were deployed with public endpoints which were visible on the open internet. In this stage, the unsecured environment was monitored by Microsoft Sentinel using logs aggregated by the Log Analytics workspace.
Architecture AFTER Hardening and Implementing Security Controls
![Azure_3](https://github.com/user-attachments/assets/545a4d4f-b31f-4b95-bf36-1c455f539780)
During the "AFTER" stage of the project, the environment was hardened and security controls were implemented in order to satisfy NIST SP 800-53 Rev4 SC-7(3) Access Points. These hardening tactics included:

    Network Security Groups (NSGs): NSGs were hardened by blocking all inbound and outbound traffic with the exception of designated public IP addresses that required access to the virtual machines. This ensured that only authorized traffic from a trusted source was allowed to access the virtual machines.

    Built-in Firewalls: Azure's built-in firewalls were configured on the virtual machines to restrict unauthorized access and protect the resources from malicious connections. This step involved fine-tuning the firewall rules based on the service and responsibilities of each VM which mitigated the attack surface bad actors had access to.

    Private Endpoints: To enhance the security of Azure Key Vault and Storage Containers, Public Endpoints were replaced with Private Endpoints. This ensured that access to these sensitive resources was limited to the virtual network and not the public internet.

Attack Maps Before Hardening / Security Controls
This attack map shows the traffic allowed by a Network Security Group with all traffic allowed inbound
![Azure_4](https://github.com/user-attachments/assets/908b1786-92ad-4f88-ac2f-79018611bc34)
This attack map shows all the attempts malicious actors attempting to access the Linux virtual machine

![Azure_5](https://github.com/user-attachments/assets/d72c8d50-2729-494d-9a82-9096b763befb)
This attack map shows all the attempts malicious actors attempting to access the Windows virtual machine

![Azure_6](https://github.com/user-attachments/assets/efacba65-c5b5-4a1f-ace9-299d348037ea)
Metrics Before Hardening / Security Controls

The following table shows the metrics measured within the unsecured environment for 24 hours: Start Time 2023-06-16 23:48:28 Stop Time 2023-03-17 23:48:28
Metric 	Count
SecurityEvent 	15953
Syslog 	3068
SecurityAlert 	52
SecurityIncident 	268
AzureNetworkAnalytics_CL 	528
Metrics After Hardening / Security Controls

All map queries actually returned no results due to no instances of malicious activity for the 24-hour period after hardening.

The following table shows the metrics measured in the environment for another 24 hours after applying security controls: Start Time 2023-06-18 17:52:43 Stop Time 2023-08-19 17:52:43
Metric 	Count
SecurityEvent 	600
Syslog 	25
SecurityAlert 	0
SecurityIncident 	0
AzureNetworkAnalytics_CL 	0
Conclusion
In this project, a mini, but effective, honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was configured to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the unsecured environment before security controls were applied and after implementing security measures. After the implementation of more robust security controls, there was a 96% reduction in Windows Security Events, a 99% reduction in Linux Events, and a 100% reduction in security alerts, incidents, and malicious inbound network traffic.

