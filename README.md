# FortiCloud
 

 As organizations increasingly adopt cloud computing for its scalability and flexibility, the
need to ensure robust security in multi-tenant environments becomes paramount. The
"FortiCloud" project aims to address this challenge by investigating and implementing
strategies for enhancing security, isolation, and access control within the Google Cloud
Platform (GCP). By leveraging innovative approaches and technologies, FortiCloud
endeavors to provide comprehensive solutions that safeguard the confidentiality,
integrity, and availability of data and resources in multi-tenant GCP environments.
Through a combination of rigorous analysis, experimentation, and implementation, this
project seeks to contribute valuable insights and best practices to the broader
cybersecurity community, enabling organizations to confidently embrace cloud
computing while minimizing security risks.

Current release include:
- YARA-L rules for [Google Security Operations](https://chronicle.security/)
- SQL queries for [BigQuery](https://cloud.google.com/bigquery/)
- SQL queries for [Log Analytics](https://cloud.google.com/logging/docs/log-analytics)

The potential security use cases below are grouped in 6 categories depending on underlying activity type and log sources:

1. :vertical_traffic_light: [Login & Access Patterns](#login-access-patterns)
2. :key: [IAM, Keys & Secrets Admin Activity](#iam-keys-secrets-changes)
3. :building_construction: [Cloud Provisoning Activity](#cloud-provisioning-activity)
4. :cloud: [Cloud Workload Usage](#cloud-workload-usage)
5. :droplet: [Data Usage](#data-usage)
6. :zap: [Network Activity](#network-activity)

 

 

## Security Analytics Use Cases
![Security Monitoring](./assets/gcp_security_mon.png)

| # | Cloud Security Threat | Log Source | Audit | Detect | ATT&CK&reg; Techniques |
|---|---|---|:-:|:-:|:-:|
| <div id="login-access-patterns">1</div> | :vertical_traffic_light: **Login & Access Patterns**
| 1.01| [Login from a highly-privileged account](./src/1.01/1.01.md)| Workspace Login Audit (Cloud Identity Logs)| | :white_check_mark:| [T1078.004](https://attack.mitre.org/techniques/T1078/004/ "Valid Accounts (Cloud Accounts)") |
| 1.02| [Suspicious login attempt flagged by Google Workspace](./src/1.02/1.02.md)| Workspace Login Audit (Cloud Identity Logs)| | :white_check_mark:| [T1078.004](https://attack.mitre.org/techniques/T1078/004/ "Valid Accounts (Cloud Accounts)") |
| 1.03| [Excessive login failures from any user identity](./src/1.03/1.03.md)| Workspace Login Audit (Cloud Identity Logs)| | :white_check_mark:| [T1078.004](https://attack.mitre.org/techniques/T1078/004/ "Valid Accounts (Cloud Accounts)"), [T1110](https://attack.mitre.org/techniques/T1110/ "Brute Force") |
| 1.10| [Access attempts violating VPC Service Controls](./src/1.10/1.10.md)| Audit Logs - Policy| :white_check_mark:| :white_check_mark:| [T1078.004](https://attack.mitre.org/techniques/T1078/004/ "Valid Accounts (Cloud Accounts)"), [T1537](https://attack.mitre.org/techniques/T1537/ "Transfer Data to Cloud Account") |
| 1.20| [Access attempts violating IAP (i.e. BeyondCorp) access controls](./src/1.20/1.20.md)| HTTP(S) LB Logs| :white_check_mark:| :white_check_mark:|  |
| 1.30| [Cloud Console accesses](./src/1.30/1.30.md)| Audit Logs - Data Access| :white_check_mark:| | [T1078.004](https://attack.mitre.org/techniques/T1078/004/ "Valid Accounts (Cloud Accounts)") |
| <div id="iam-keys-secrets-changes">2</div> | :key: **IAM, Keys & Secrets Changes**
| 2.02| [User added to highly-privileged Google Group](./src/2.02/2.02.md)| Workspace Admin Audit| :white_check_mark:| :white_check_mark:| [T1078.004](https://attack.mitre.org/techniques/T1078/004/ "Valid Accounts (Cloud Accounts)"), [T1484.001](https://attack.mitre.org/techniques/T1484/001/ "Domain Policy Modification (Group Policy Modification)") |
| 2.20| [Permissions granted over a Service Account](./src/2.20/2.20.md)| Audit Logs - Admin Activity| :white_check_mark:| :white_check_mark:| [T1484.002](https://attack.mitre.org/techniques/T1484/002/ "Domain Policy Modification (Domain Trust Modification)") |
| 2.21| [Permissions granted to impersonate Service Account](./src/2.21/2.21.md)| Audit Logs - Admin Activity| :white_check_mark:| :white_check_mark:| [T1484.002](https://attack.mitre.org/techniques/T1484/002/ "Domain Policy Modification (Domain Trust Modification)") |
| 2.22| [Permissions granted to create or manage Service Account keys](./src/2.22/2.22.md)| Audit Logs - Admin Activity| :white_check_mark:| :white_check_mark:| [T1484.002](https://attack.mitre.org/techniques/T1484/002/ "Domain Policy Modification (Domain Trust Modification)") |
| 2.30| [Service accounts or keys created by non-approved identity](./src/2.30/2.30.md)| Audit Logs - Admin Activity| :white_check_mark:| :white_check_mark:| [T1136.003](https://attack.mitre.org/techniques/T1136/003/ "Create Account (Cloud Account)") |
| 2.40| [User access added (or removed) from IAP-protected HTTPS services](./src/2.40/2.40.md)| Audit Logs - Admin Activity| :white_check_mark:| :white_check_mark:| [T1484.002](https://attack.mitre.org/techniques/T1484/002/ "Domain Policy Modification (Domain Trust Modification)") |
| <div id="cloud-provisioning-activity">3</div> | :building_construction: **Cloud Provisioning Activity**
| 3.01| [Changes made to logging settings](./src/3.01/3.01.md)| Audit Logs - Admin Activity| :white_check_mark:| :white_check_mark:| [T1562.008](https://attack.mitre.org/techniques/T1562/008/ "Impair Defenses (Disable Cloud Logs)") |
| 3.02| [Disabling VPC Flows logging](./src/3.02/3.02.md)| Audit Logs - Admin Activity| | :white_check_mark:| [T1562.008](https://attack.mitre.org/techniques/T1562/008/ "Impair Defenses (Disable Cloud Logs)") |
| 3.11| [Unusual number of firewall rules modified in the last 7 days](./src/3.11/3.11.md)| Audit Logs - Admin Activity| | :white_check_mark:| [T1562.007](https://attack.mitre.org/techniques/T1562/007/ "Impair Defenses (Disable or Modify Cloud Firewall)") |
| 3.12| [Firewall rules modified or deleted in the last 24 hrs](./src/3.12/3.12.md)| Audit Logs - Admin Activity| :white_check_mark:| :white_check_mark:| [T1562.007](https://attack.mitre.org/techniques/T1562/007/ "Impair Defenses (Disable or Modify Cloud Firewall)") |
| 3.13| [VPN tunnels created or deleted](./src/3.13/3.13.md)| Audit Logs - Admin Activity| :white_check_mark:| :white_check_mark:| [T1133](https://attack.mitre.org/techniques/T1133/ "External Remote Services") |
| 3.14| [DNS zones modified or deleted](./src/3.14/3.14.md)| Audit Logs - Admin Activity| :white_check_mark:| :white_check_mark:| [T1578](https://attack.mitre.org/techniques/T1578/ "Modify Cloud Compute Infrastructure") |
| 3.15| [Cloud Storage buckets modified or deleted by unfamiliar user identities](./src/3.15/3.15.md)| Audit Logs - Admin Activity| :white_check_mark:| :white_check_mark:| [T1578](https://attack.mitre.org/techniques/T1578/ "Modify Cloud Compute Infrastructure") |
| 3.20| [VMs deleted in the last 7 days](./src/3.20/3.20.md)| Audit Logs - Admin Activity| :white_check_mark:| | [T1578](https://attack.mitre.org/techniques/T1578/ "Modify Cloud Compute Infrastructure") |
| 3.21| [Cloud SQL databases created, modified or deleted](./src/3.21/3.21.md)| Audit Logs - Admin Activity| :white_check_mark:| | [T1578](https://attack.mitre.org/techniques/T1578/ "Modify Cloud Compute Infrastructure") |
| <div id="cloud-workload-usage">4</div> | :cloud: **Cloud Workload Usage**
| 4.01| [Unusually high API usage by any user identity](./src/4.01/4.01.md)| Audit Logs| :white_check_mark:| :white_check_mark:| [T1106](https://attack.mitre.org/techniques/T1106/ "Native API") |
| 4.10| [Autoscaling usage in the past month](./src/4.10/4.10.md)| Audit Logs - Admin Activity| :white_check_mark:| | [T1496](https://attack.mitre.org/techniques/T1496/ "Resource Hijacking") |
| 4.11| [Autoscaling usage per day in the past month](./src/4.11/4.11.md)| Audit Logs - Admin Activity| :white_check_mark:| | [T1496](https://attack.mitre.org/techniques/T1496/ "Resource Hijacking") |
| 4.20| [Resource access by certain user identities in the past month](./src/4.20/4.20.md)| Audit Logs| :white_check_mark:| | [T1106](https://attack.mitre.org/techniques/T1106/ "Native API") |
| 4.21| [Resource access by certain user identities in the past month (aggregated by day)](./src/4.21/4.21.md)| Audit Logs| :white_check_mark:| | [T1106](https://attack.mitre.org/techniques/T1106/ "Native API") |
| 4.30| [Which users most frequently used LLM models?](./src/4.30/4.30.md)| Audit Logs - Data Access| :white_check_mark:| :white_check_mark:| [T1496](https://attack.mitre.org/techniques/T1496/ "Resource Hijacking"), [AML.T0051](https://atlas.mitre.org/techniques/AML.T0051 "LLM Prompt Injection"), [AML.T0057](https://atlas.mitre.org/techniques/AML.T0057 "LLM Data Leakage") |
| 4.31| [Usage of LLM models over time](./src/4.31/4.31.md)| Audit Logs - Data Access| :white_check_mark:| :white_check_mark:| [T1496](https://attack.mitre.org/techniques/T1496/ "Resource Hijacking"), [AML.T0051](https://atlas.mitre.org/techniques/AML.T0051 "LLM Prompt Injection"), [AML.T0057](https://atlas.mitre.org/techniques/AML.T0057 "LLM Data Leakage") |
| <div id="data-usage">5</div> | :droplet: **Data Usage**
| 5.01| [Which users most frequently accessed data in the past week?](./src/5.01/5.01.md)| Audit Logs - Data Access| :white_check_mark:| | [T1530](https://attack.mitre.org/techniques/T1530/ "Data from Cloud Storage Object") |
| 5.02| [Which users accessed most amount of data in the past week?](./src/5.02/5.02.md)| Audit Logs - Data Access| :white_check_mark:| | [T1530](https://attack.mitre.org/techniques/T1530/ "Data from Cloud Storage Object") |
| 5.03| [How much data was accessed by each user per day in the past week?](./src/5.03/5.03.md)| Audit Logs - Data Access| :white_check_mark:| | [T1530](https://attack.mitre.org/techniques/T1530/ "Data from Cloud Storage Object") |
| 5.04| [Which users accessed data in a given table in the past month?](./src/5.04/5.04.md)| Audit Logs - Data Access| :white_check_mark:| | [T1078.004](https://attack.mitre.org/techniques/T1078/004/ "Valid Accounts (Cloud Accounts)") |
| 5.05| [What tables are most frequently accessed and by whom?](./src/5.05/5.05.md)| Audit Logs - Data Access| :white_check_mark:| | [T1530](https://attack.mitre.org/techniques/T1530/ "Data from Cloud Storage Object") |
| 5.06| [Top 10 queries against BigQuery in the past week](./src/5.06/5.06.md)| Audit Logs - Data Access| :white_check_mark:| | [T1530](https://attack.mitre.org/techniques/T1530/ "Data from Cloud Storage Object") |
| 5.07| [Any queries doing very large scans?](./src/5.07/5.07.md)| Audit Logs - Data Access| :white_check_mark:| :white_check_mark:| [T1530](https://attack.mitre.org/techniques/T1530/ "Data from Cloud Storage Object") |
| 5.08| [Any destructive queries or jobs (i.e. update or delete)?](./src/5.08/5.08.md)| Audit Logs| :white_check_mark:| :white_check_mark:| [T1565.001](https://attack.mitre.org/techniques/T1565/001/ "Data Manipulation (Stored Data Manipulation)") |
| 5.10| [Recent data read with granular access and permissions details](./src/5.10/5.10.md)| Audit Logs - Data Access| :white_check_mark:| | [T1074](https://attack.mitre.org/techniques/T1074/ "Data Staged"), [T1213](https://attack.mitre.org/techniques/T1213/ "Data from Information Repositories") |
| 5.11| [Recent dataset activity with granular permissions details](./src/5.11/5.11.md)| Audit Logs - Admin Activity| :white_check_mark:| | [T1074](https://attack.mitre.org/techniques/T1074/ "Data Staged"), [T1213](https://attack.mitre.org/techniques/T1213/ "Data from Information Repositories") |
| 5.20| [Most common data (and metadata) access actions in the past month](./src/5.20/5.20.md)| Audit Logs - Data Access| :white_check_mark:| :white_check_mark:| [T1530](https://attack.mitre.org/techniques/T1530/ "Data from Cloud Storage Object") |
| 5.30| [Cloud Storage buckets enumerated by unfamiliar user identities](./src/5.30/5.30.md)| Audit Logs - Data Access| :white_check_mark:| :white_check_mark:| [T1530](https://attack.mitre.org/techniques/T1530/ "Data from Cloud Storage Object") |
| 5.31| [Cloud Storage objects accessed from a new IP](./src/5.31/5.31.md)| Audit Logs - Data Access| :white_check_mark:| :white_check_mark:| [T1530](https://attack.mitre.org/techniques/T1530/ "Data from Cloud Storage Object") |
| <div id="network-activity">6</div> | :zap: **Network Activity**
| 6.01| [Hosts reaching out to many other hosts or ports per hour](./src/6.01/6.01.md)| VPC Flow Logs| :white_check_mark:| :white_check_mark:| [T1046](https://attack.mitre.org/techniques/T1046/ "Network Service Scanning") |
| 6.10| [Connections from a new IP to an in-scope network](./src/6.10/6.10.md)| VPC Flow Logs| :white_check_mark:| :white_check_mark:| [T1018](https://attack.mitre.org/techniques/T1018/ "Remote System Discovery") |
| 6.15| [List all IP addresses with any associated entities](./src/6.15/6.15.md)| VPC Flow Logs| :white_check_mark:| | [T1018](https://attack.mitre.org/techniques/T1018/ "Remote System Discovery"), [T1046](https://attack.mitre.org/techniques/T1046/ "Network Service Scanning") |
| 6.20| [Connections blocked by Cloud Armor](./src/6.20/6.20.md)| HTTP(S) LB Logs| :white_check_mark:| :white_check_mark:| [T1071](https://attack.mitre.org/techniques/T1071/ "Application Layer Protocol") |
| 6.21| [Log4j 2 vulnerability exploit attempts](./src/6.21/6.21.md)| HTTP(S) LB Logs| | :white_check_mark:| [T1190](https://attack.mitre.org/techniques/T1190/ "Exploit Public-Facing Application") |
| 6.22| [Any remote IP addresses attempting to exploit Log4j 2 vulnerability?](./src/6.22/6.22.md)| HTTP(S) LB Logs| | :white_check_mark:| [T1190](https://attack.mitre.org/techniques/T1190/ "Exploit Public-Facing Application") |
| 6.23| [Spring4Shell vulnerability exploit attempts (CVE-2022-22965)](./src/6.23/6.23.md)| HTTP(S) LB Logs| | :white_check_mark:| [T1190](https://attack.mitre.org/techniques/T1190/ "Exploit Public-Facing Application") |
| 6.30| [Virus or malware detected by Cloud IDS](./src/6.30/6.30.md)| Cloud IDS Threat Logs| | :white_check_mark:| [T1059](https://attack.mitre.org/techniques/T1059/ "Command and Scripting Interpreter") |
| 6.31| [Traffic sessions of high severity threats detected by Cloud IDS](./src/6.31/6.31.md)| Cloud IDS Threat Logs, Cloud IDS Traffic Logs| | :white_check_mark:| [T1071](https://attack.mitre.org/techniques/T1071/ "Application Layer Protocol") |
| 6.40| [Top 10 DNS queried domains](./src/6.40/6.40.md)| Cloud DNS Logs| :white_check_mark:| :white_check_mark:| [T1071.004](https://attack.mitre.org/techniques/T1071/004/ "Command and Scripting Interpreter (Unix Shell)") |

The documentation of the risk assessment,security analysis and architectural design of multi-tenancy in gcp environments is shared below:
   https://medium.com/@riyasanvi2002/forticloud-b46224aebd4e


   To Sum Up Multi-tenant Architecture:
The adoption of multi-tenant architecture (MTA) for cloud apps presents a transformative opportunity for businesses seeking to harness the power of cloud computing. By allowing multiple tenants to share the same infrastructure, applications, and databases, MTA offers a cost-effective, scalable, and flexible solution that can accommodate the evolving needs of a diverse user base.

As cloud technology continues to advance, we can expect even more sophisticated security measures to emerge, further enhancing the appeal of multi-tenant architecture.

The adoption of multi-tenant architecture for cloud apps represents a strategic move for businesses aiming to stay competitive in the digital age. It offers a pathway to increased efficiency, agility, and innovation, enabling organizations to deliver high-quality services while maintaining control over their costs and resources. 
