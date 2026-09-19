# Attack Simulation - APT Healthcare Data Theft

## 1. Scenario Overview

This simulation models a hypothetical advanced persistent threat (APT) targeting Solaris Care Connect 360.

The objective of the attacker is to obtain unauthorized access to the healthcare platform and exfiltrate patient records.

### Attacker Objective

**Steal patient records from Solaris Care Connect 360.**

### Simulated Impact

- Unauthorized access to healthcare systems
- Compromise of privileged credentials
- Lateral movement within the environment
- Unauthorized access to patient records
- Exfiltration of approximately 50,000 patient records
- Public exposure of stolen information
- Delayed breach detection

> This is a controlled threat-modeling scenario. The timeline represents a hypothetical attack path and does not indicate that Solaris Care Connect 360 has actually experienced such an incident.

---

# 2. Attack Timeline

| Day | Attacker Activity | Security Stage | Potential Detection |
|-----|-------------------|-----------------|---------------------|
| Day 1 | Researches Solaris employees on LinkedIn | Reconnaissance | Threat intelligence / OSINT monitoring |
| Day 3 | Identifies IT administrator email addresses | Reconnaissance | External exposure review |
| Day 5 | Sends targeted phishing email | Initial Access | Email security gateway |
| Day 5 | IT administrator clicks link and credentials are stolen | Credential Access | Endpoint and identity monitoring |
| Day 6 | Attacker logs in through VPN using stolen credentials | Initial Access | Authentication anomaly detection |
| Day 7 | Attacker moves laterally toward database server | Lateral Movement | Network monitoring / EDR |
| Day 8 | Attacker exports approximately 50,000 patient records | Collection / Exfiltration | Database monitoring / DLP |
| Day 9 | Stolen data appears on dark web | Exfiltration / Impact | Threat intelligence monitoring |
| Day 30 | Breach discovered during audit | Detection | Audit / investigation |

---

# 3. Day-by-Day Attack Walkthrough

## Day 1 - Reconnaissance

### Attacker Activity

The attacker researches Solaris Care Connect 360 and its employees using publicly available information.

Potential information of interest includes:

- Organization structure
- Technology roles
- Employee names
- Public contact information
- Job descriptions
- Technology used by the organization

### Threat Modeling Mapping

**Cyber Kill Chain:** Reconnaissance

**MITRE ATT&CK:** Reconnaissance

**Related Risk:** S1 - Credential Phishing

### Defensive Opportunities

The organization should minimize unnecessary exposure of sensitive organizational information.

Potential controls:

- Security awareness training
- External attack-surface monitoring
- Review of publicly exposed employee information
- Threat intelligence monitoring

---

# Day 3 - Target Identification

### Attacker Activity

The attacker identifies employees who appear to have IT administration responsibilities and collects their publicly available email addresses.

The attacker selects an administrator as the target because compromise of a privileged account could provide access to more systems.

### Security Concern

Privileged accounts represent a higher-value target because they may have access to sensitive infrastructure.

### Defensive Opportunities

Controls include:

- Privileged account separation
- MFA
- Phishing-resistant authentication
- Least privilege
- Monitoring of privileged accounts

---

# Day 5 - Targeted Phishing

### Attacker Activity

The attacker sends a targeted phishing email to the selected administrator.

The message is designed to persuade the recipient to interact with a malicious link.

### Potential Detection

A secure email gateway could identify suspicious characteristics of the message.

Endpoint security could also detect suspicious browser or process activity following the interaction.

### Controls

- Secure email gateway
- URL filtering
- Malware and phishing detection
- Security awareness training
- Endpoint Detection and Response (EDR)

### Related Risk

**S1 - Credential Phishing**

Risk Score: **16 - Critical**

---

# Day 5 - Credential Theft

### Attacker Activity

The administrator interacts with the phishing link and their credentials are compromised.

The attacker now possesses authentication material that may allow access to organizational systems.

### Defensive Opportunities

Multi-factor authentication can provide an additional authentication requirement beyond the stolen password.

Phishing-resistant authentication can further reduce the usefulness of stolen credentials.

### Controls

- MFA
- Phishing-resistant authentication
- Conditional access
- Suspicious-login detection
- Credential monitoring

---

# Day 6 - VPN Access

### Attacker Activity

The attacker attempts to use the compromised credentials to authenticate through the organization's VPN.

### Potential Detection

The authentication system should evaluate the login for unusual characteristics such as:

- New device
- Unusual location
- Unusual login time
- Abnormal authentication behavior
- Impossible travel patterns
- Unexpected privileged-account activity

### Controls

- MFA
- Conditional access
- VPN authentication monitoring
- Identity and Access Management (IAM)
- Security Information and Event Management (SIEM)

### Detection Opportunity

**High-value detection point**

If the organization detects and blocks the anomalous authentication here, the simulated attack chain can be stopped before lateral movement.

---

# Day 7 - Lateral Movement

### Attacker Activity

The attacker attempts to move from the initially compromised system toward systems containing sensitive patient information.

The target is the database infrastructure supporting patient records.

### Security Concern

Compromise of one endpoint should not automatically provide unrestricted access to other systems.

### Defensive Controls

- Network segmentation
- Least privilege
- Privileged Access Management (PAM)
- Endpoint Detection and Response
- Network Detection and Response
- Internal firewall rules
- Database access restrictions

### Detection Opportunity

Unusual connections between an administrator workstation or VPN session and database infrastructure should generate security alerts.

---

# Day 8 - Patient Record Collection

### Attacker Activity

The attacker gains access to the patient-record environment and attempts to collect a large volume of patient information.

The simulation assumes approximately:

**50,000 patient records**

are accessed and exported.

### Potential Detection

A large export of patient records should be considered anomalous and trigger investigation.

Potential indicators include:

- Unusually large database queries
- Bulk record access
- Large data exports
- Access outside normal working patterns
- Unusual database account activity
- Unexpected administrative queries

### Controls

- Database Activity Monitoring
- Data Loss Prevention
- SIEM
- Database access logging
- User and Entity Behavior Analytics
- Least-privilege database access

### Related Risks

**I1 - SQL Injection / Data Breach**

Risk Score: **20 - Critical**

**I2 - Excessive Data Exposure**

Risk Score: **12 - High**

---

# Day 9 - Data Appears on Dark Web

### Attacker Activity

The simulation assumes the stolen information is subsequently made available through an illicit online marketplace or forum.

### Defensive Opportunity

External threat intelligence monitoring may identify indicators that stolen organizational or patient information has appeared online.

Potential controls:

- Threat intelligence
- Dark-web monitoring
- Incident response procedures
- Data exposure monitoring

### Important Limitation

Discovery of information online occurs after the simulated data theft.

Therefore, this control should be considered a detection and response mechanism rather than a preventative control.

---

# Day 30 - Breach Discovery

### Discovery

The simulated breach is discovered during an audit approximately 30 days after the initial compromise.

### Security Finding

The 30-day detection period demonstrates a significant detection gap in the hypothetical scenario.

The most important lesson is that multiple opportunities for earlier detection existed between the initial phishing event and the eventual audit discovery.

Potential earlier detection points include:

1. Phishing email
2. Credential theft
3. Abnormal VPN authentication
4. Lateral movement
5. Database access
6. Large-scale data export

---

# 4. Attack Chain

```mermaid
flowchart LR
    A[Reconnaissance] --> B[Target Identification]
    B --> C[Phishing]
    C --> D[Credential Theft]
    D --> E[VPN Access]
    E --> F[Lateral Movement]
    F --> G[Database Access]
    G --> H[Patient Record Collection]
    H --> I[Data Exfiltration]
    I --> J[External Exposure]
    J --> K[Detection During Audit]