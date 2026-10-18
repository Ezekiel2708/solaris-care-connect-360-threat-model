# Solaris Care Connect 360 — Threat Modeling & Security Assessment

![Status](https://img.shields.io/badge/Project-Portfolio-blue)
![Focus](https://img.shields.io/badge/Focus-Threat%20Modeling-red)
![Framework](https://img.shields.io/badge/Framework-STRIDE-orange)
![ATT%26CK](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-purple)

## Project Overview

**Solaris Care Connect 360** is a fictional healthcare platform used for a structured threat-modeling and security assessment project.

The platform manages sensitive patient information and supports real-time vital-sign monitoring, prescription management, doctor–patient messaging, and integration with external healthcare providers.

This project applies established cybersecurity frameworks to identify potential threats, assess risk, evaluate security controls, and develop a prioritized remediation roadmap.

> **Disclaimer:** Solaris Care Connect 360 is a fictional system created for educational and portfolio purposes. The architecture, threats, attack scenarios, risk scores, and control statuses are illustrative assessment assumptions, not findings from a real healthcare organization or production environment.

## Objectives

* Model the system architecture and data flows.
* Identify threats using the STRIDE methodology.
* Map selected threats to MITRE ATT&CK techniques.
* Analyze representative attack scenarios using the Cyber Kill Chain.
* Build attack trees to illustrate potential attack paths.
* Assess and prioritize risks using likelihood and impact.
* Map security controls to identified threats.
* Develop a remediation plan with priorities, ownership, and timelines.
* Communicate findings through technical reports and an executive summary.

## System Scope

The modeled platform includes:

| Layer                 | Components                                                                                            |
| --------------------- | ----------------------------------------------------------------------------------------------------- |
| Frontend              | Patient mobile application, doctor web portal, administrator dashboard                                |
| Backend               | API gateway, authentication service, patient records service, prescription service, messaging service |
| Data                  | PostgreSQL patient database, S3 document storage, Redis cache                                         |
| External integrations | Insurance providers, pharmacy networks, laboratory systems                                            |

### Key Assumptions

* The platform manages approximately 50,000 active patient records.
* Patient health information and prescription data are sensitive assets.
* External integrations exchange healthcare-related information with the platform.
* The system requires appropriate authentication, authorization, audit logging, and protection of sensitive data.
* Risk scores and control statuses are based on the assumptions documented in this project.

## Methodologies and Frameworks

### 1. STRIDE Threat Modeling

STRIDE is used to categorize threats across six areas:

| Category               | Security concern                                 |
| ---------------------- | ------------------------------------------------ |
| Spoofing               | Impersonation of users or systems                |
| Tampering              | Unauthorized modification of data                |
| Repudiation            | Disputes about actions without adequate evidence |
| Information Disclosure | Unauthorized exposure of sensitive information   |
| Denial of Service      | Disruption of system availability                |
| Elevation of Privilege | Unauthorized acquisition of higher privileges    |

The threat analysis documents **31 potential threats** across these categories.

### 2. MITRE ATT&CK

Selected threats are mapped to relevant MITRE ATT&CK techniques to connect the threat model with known adversary behaviors.

The mapping includes potential credential theft, exploitation of public-facing applications, and abuse of valid accounts. Associated mitigations are documented where applicable.

### 3. Cyber Kill Chain

Representative attack scenarios are analyzed through the Cyber Kill Chain to identify opportunities for prevention, detection, and response.

Scenarios include:

* A hypothetical ransomware attack.
* An insider threat involving misuse of legitimate access.
* A simulated healthcare data theft scenario.

### 4. Risk Assessment

Risks are evaluated using a likelihood-and-impact model:

**Risk Score = Likelihood × Impact**

The project uses a five-point scale for likelihood and impact. Scores are then classified using the documented risk matrix.

These scores are illustrative prioritization estimates rather than measured probabilities of compromise.

## Key Risk Assessment Results

The detailed risk register contains eight prioritized risks:

| Risk ID | Threat                                 | Score | Severity |
| ------- | -------------------------------------- | ----: | -------- |
| I1      | SQL Injection — Patient Data Exposure  |    20 | Critical |
| S1      | Credential Phishing                    |    16 | Critical |
| E3      | SQL Injection Leading to DBA Access    |    15 | High     |
| E1      | Patient-to-Doctor Privilege Escalation |    12 | High     |
| I2      | Excessive Data Exposure                |    12 | High     |
| T2      | Prescription Tampering                 |    10 | High     |
| D1      | Denial-of-Service Attack               |     9 | Medium   |
| R1      | Prescription Approval Denial           |     6 | Medium   |

**Risk distribution:** 2 Critical, 4 High, 2 Medium, and 0 Low.

These classifications reflect the project's detailed risk register. The original STRIDE severity labels may differ because they represent initial threat-level estimates rather than the detailed likelihood-and-impact assessment.

## Recommended Security Controls

The remediation plan prioritizes the following improvements:

* **Secure database queries:** Use parameterized queries, input validation, and appropriate database permissions.
* **Stronger authentication:** Enforce multi-factor authentication and improve detection of suspicious login activity.
* **Least privilege:** Restrict database, application, and administrative permissions to necessary operations.
* **API security:** Enforce server-side authorization and return only the data required by each request.
* **Prescription integrity:** Implement prescriber authorization, approval workflows, and protected audit trails.
* **Monitoring and detection:** Improve database activity monitoring, access anomaly detection, and logging.
* **Availability protection:** Apply rate limiting, request throttling, and appropriate denial-of-service protections.
* **Security assurance:** Conduct vulnerability assessments, penetration testing, and periodic access reviews.

### Remediation Priorities

| Timeline    | Focus                                                                                        |
| ----------- | -------------------------------------------------------------------------------------------- |
| 0–30 days   | Address the highest-scoring risks, strengthen authentication, and review database privileges |
| 31–90 days  | Improve API authorization, prescription integrity, monitoring, and availability protections  |
| 91–180 days | Mature security monitoring, testing, governance, and continuous improvement                  |

Timelines are proposed planning targets, not evidence that the controls have been implemented.

## Project Documentation

### Architecture and Diagrams

* [System Architecture](diagrams/architecture.md)
* [Data Flow Diagram — Level 0](diagrams/dfd-level0.md)
* [Data Flow Diagram — Level 1](diagrams/dfd-level1.md)
* [Attack Trees](diagrams/attack-trees.md)

### Reports

* [Executive Summary](reports/executive-summary.md)
* [Full Threat Model Report](reports/threat-model-report.md)
* [STRIDE Threat Analysis](reports/stride-threats.md)
* [Risk Assessment](reports/risk-assesment.md)
* [Remediation Plan](reports/remediation-plan.md)
* [Attack Simulation](reports/attack-simulation.md)

### Reusable Templates

* [STRIDE Threat Template](templates/stride-threats.md)
* [MITRE ATT&CK Mapping Template](templates/mitre-mapping.md)
* [Cyber Kill Chain Analysis Template](templates/kill-chain-analysis.md)
* [Risk Register Template](templates/risk-register.md)
* [Security Control Mapping Template](templates/control-mapping.md)

## Skills Demonstrated

* Threat modeling and attack-path analysis
* Security architecture review
* Data-flow and trust-boundary analysis
* STRIDE threat identification
* MITRE ATT&CK mapping
* Risk assessment and prioritization
* Security control mapping
* Remediation planning
* Incident detection and response planning
* Technical security documentation

## Limitations

This is a portfolio project based on a fictional system. No real patient data was used, and the attack scenarios are hypothetical.

The risk ratings, control statuses, and remediation timelines are assessment assumptions. They do not establish that a real system is vulnerable or that any listed control has been deployed and verified.

The project demonstrates a structured assessment approach; it is not a certification of compliance, a production security audit, or proof of implemented security controls.

## Author

**Ucheomachukwu Ezekiel-Onichakwe**

Cybersecurity | Threat Modeling | Security Assessment | DevSecOps

* [GitHub Profile](https://github.com/Ezekiel2708)
* [Portfolio Website](https://ezekiel2708.github.io/portifolio/)
* [LinkedIn](https://www.linkedin.com/in/ucheomachukwu-ezekiel-onichakwe-082a30342)

---

*Developed as a cybersecurity portfolio project for educational and professional development.*
