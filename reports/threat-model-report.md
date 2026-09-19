# Threat Model Report
## Solaris Care Connect 360

**Version:** 1.0  
**Date:** 18 September 2026  
**Author:** Ucheomachukwu Ezekiel-Onichakwe  
**Classification:** Confidential

---

## Executive Summary

Solaris Care Connect 360 is a healthcare platform designed to manage patient records, real-time vital signs monitoring, prescription management, doctor-patient messaging, and integrations with insurance providers, pharmacy networks, and laboratory systems. The platform manages approximately 50,000 active patient records and therefore requires strong controls to protect sensitive healthcare and personal information.

The system was analyzed using **STRIDE threat modeling**, **MITRE ATT&CK technique mapping**, and **Cyber Kill Chain analysis**. The identified threats were subsequently assessed using a likelihood × impact risk-scoring methodology.

### Key Findings

| Severity | Count | Key Examples |
|----------|-------|--------------|
| **Critical** | 2 | SQL Injection (I1), Credential Phishing (S1) |
| **High** | 4 | SQLi to DBA (E3), Privilege Escalation (E1) |
| **Medium** | 2 | DDoS (D1), Repudiation (R1) |
| **Low** | 0 | - |
| **Total** | **8** | |

### Critical Recommendations

1. **Implement Multi-Factor Authentication (MFA)** - Strengthens authentication and reduces the risk of compromised credentials being used to access the platform.
2. **Deploy a Web Application Firewall (WAF)** - Provides an additional preventive layer against SQL injection and other malicious web requests.
3. **Enable Database Activity Monitoring** - Detects suspicious database queries and unauthorized access to sensitive patient information.

### Risk Summary

The assessment identified significant gaps in preventive and detective security controls. In particular, SQL injection protection, authentication security, database privilege management, and security monitoring require additional controls.

Without remediation, these gaps could increase the likelihood and potential impact of unauthorized access, data exposure, data manipulation, and service disruption.

---

## 1. Scope & Methodology

### 1.1 Scope

The threat modeling assessment covered the following components of Solaris Care Connect 360:

- Patient-facing mobile application for iOS and Android
- Doctor web portal
- Administrator dashboard
- API Gateway
- Authentication Service
- Patient Records Service
- Prescription Service
- Messaging Service
- Vitals Monitoring Service
- PostgreSQL patient database
- Document storage
- Redis cache
- Insurance provider APIs
- Pharmacy network integrations
- Laboratory results feeds

### 1.2 Methodology

The assessment used multiple complementary security frameworks:

- **STRIDE** - Used to categorize threats involving Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, and Elevation of Privilege.
- **MITRE ATT&CK** - Used to map selected threats to adversary techniques and tactics.
- **Cyber Kill Chain** - Used to analyze how attacks could progress from reconnaissance through exploitation and eventual impact.
- **Likelihood × Impact Risk Scoring** - Used to assign risk scores and determine risk severity.

### 1.3 Out of Scope

The following areas were outside the scope of this assessment:

- Physical security
- Third-party vendor assessments
- Penetration testing
- Physical infrastructure security
- Detailed source-code review

---

## 2. System Overview

Solaris Care Connect 360 consists of client applications, backend services, databases, storage systems, and external healthcare-related integrations.

### 2.1 Architecture

The main architecture consists of:

- **External Users:** Patient Mobile App, Doctor Web Portal, and Admin Dashboard
- **API Layer:** API Gateway and Authentication Service
- **Backend Services:** Patient Records, Prescription, Messaging, and Vitals Monitoring
- **Data Layer:** PostgreSQL, Document Storage, and Redis Cache
- **External Systems:** Insurance Provider APIs, Pharmacy Networks, and Laboratory Systems

For the detailed architecture diagram, see:

[Architecture Diagram](../diagrams/architecture.md)

### 2.2 Trust Boundaries

The assessment identified four major trust boundaries:

1. **Internet → API Gateway**
   - Untrusted external traffic enters the application through the API Gateway.

2. **API Gateway → Backend Services**
   - Requests are routed to internal application services and must be authenticated and authorized.

3. **Backend Services → Database and Storage**
   - Application services access sensitive patient records, prescriptions, documents, and other protected data.

4. **Backend Services → External APIs**
   - The platform exchanges information with insurance providers, pharmacy networks, and laboratory systems.

### 2.3 Sensitive Data Flows

The primary sensitive data handled by the platform includes:

- Patient Protected Health Information (PHI)
- Patient medical records
- Real-time vital signs
- Prescription information
- Authentication credentials
- Patient demographic information
- Insurance information
- Insurance and financial data
- Laboratory results
- Doctor-patient communications

Sensitive data should be protected through appropriate authentication, authorization, encryption, logging, and access-control mechanisms.

### 2.4 Data Flow Diagrams

Detailed data flow analysis is available in:

- [Level 0 Data Flow Diagram](../diagrams/dfd-level0.md)
- [Level 1 Data Flow Diagram](../diagrams/dfd-level1.md)

---

## 3. Threat Analysis

### 3.1 STRIDE Summary

The STRIDE analysis identified 31 individual threats across six threat categories.

| Category | Threat Count | Critical | High |
|----------|--------------|----------|------|
| Spoofing | 5 | 0 | 2 |
| Tampering | 5 | 2 | 2 |
| Repudiation | 5 | 0 | 2 |
| Information Disclosure | 6 | 2 | 2 |
| Denial of Service | 5 | 0 | 2 |
| Elevation of Privilege | 5 | 3 | 1 |
| **Total** | **31** | **7** | **11** |

The complete STRIDE threat register is available in:

- [STRIDE Analysis](./stride-analysis.md)
- [Complete STRIDE Threat Register](./stride-threats.md)

### 3.2 Key STRIDE Threats

Important threats identified during the assessment include:

- Credential theft through phishing
- Fake or unauthorized doctor accounts
- Forged or stolen authentication tokens
- Unauthorized modification of patient records
- Prescription alteration
- SQL injection exposing patient records
- Excessive API data exposure
- Unencrypted sensitive data transmission
- Database resource exhaustion
- Patient-to-doctor privilege escalation
- Doctor-to-administrator privilege escalation
- SQL injection leading to elevated database access
- Container escape
- Insecure direct object references (IDOR)

### 3.3 MITRE ATT&CK Mapping

Selected threats were mapped to MITRE ATT&CK techniques to describe potential adversary behavior.

Key techniques identified include:

- **T1566 - Phishing** - Initial Access
- **T1190 - Exploit Public-Facing Application** - Initial Access
- **T1078 - Valid Accounts** - Credential Access / Persistence
- **T1528 - Steal Application Access Token** - Credential Access
- **T1567 - Exfiltration Over Web Service** - Exfiltration

The detailed mapping and recommended mitigations are available in:

[MITRE ATT&CK Mapping](./mitre-mapping.md)

### 3.4 Cyber Kill Chain Analysis

Three major attack scenarios were considered:

1. **Ransomware via phishing**
   - Reconnaissance
   - Weaponization
   - Delivery
   - Exploitation
   - Installation
   - Command and Control
   - Actions on Objectives

2. **Insider data theft**
   - Existing employee access
   - Privilege abuse
   - Use of legitimate tools
   - Patient data collection and export

3. **External SQL injection attack**
   - Reconnaissance of the public-facing application
   - Exploitation of an SQL injection vulnerability
   - Database access
   - Data discovery and potential exfiltration

The detailed analysis is available in:

[Cyber Kill Chain Analysis](./cyber-kill-chain.md)

---

## 4. Risk Assessment

Risk was assessed using the following formula:

**Risk = Likelihood × Impact**

Likelihood and impact were evaluated on a scale from 1 to 5. The resulting scores were used to categorize risks as Low, Medium, High, or Critical.

### 4.1 Risk Distribution

| Risk Level | Score Range | Number |
|------------|-------------|--------|
| Critical | 16–25 | 2 |
| High | 10–15 | 4 |
| Medium | 5–9 | 2 |
| Low | 1–4 | 0 |

### 4.2 Risk Matrix

| Likelihood ↓ / Impact → | 1 | 2 | 3 | 4 | 5 |
|-------------------------|---:|---:|---:|---:|---:|
| **5 - Almost Certain** | 5 M | 10 H | 15 H | 20 C | 25 C |
| **4 - Likely** | 4 L | 8 M | 12 H | 16 C | 20 C |
| **3 - Possible** | 3 L | 6 M | 9 M | 12 H | 15 H |
| **2 - Unlikely** | 2 L | 4 L | 6 M | 8 M | 10 H |
| **1 - Rare** | 1 L | 2 L | 3 L | 4 L | 5 M |

**Legend:** L = Low, M = Medium, H = High, C = Critical

### 4.3 Top 5 Risks

| Risk ID | Risk | Score | Severity |
|---------|------|------:|----------|
| **I1** | SQL Injection - Data Breach | 20 | Critical |
| **S1** | Credential Phishing | 16 | Critical |
| **E3** | SQL Injection to DBA Access | 15 | High |
| **I2** | Excessive Data Return | 12 | High |
| **E1** | Patient-to-Doctor Privilege Escalation | 12 | High |

Detailed risk calculations are available in:

- [Risk Assessment](./risk-assessment.md)
- [Risk Register](./risk-register.md)

---

## 5. Security Controls & Recommendations

The assessment compared existing controls against required controls using three control-status categories:

| Status | Definition |
|--------|------------|
| **Implemented** | The control is in place, properly configured, and considered effective against the identified risk. |
| **Partial** | The control exists, but its coverage, configuration, enforcement, or effectiveness needs improvement. |
| **Missing** | No control is currently in place to address the identified risk. |

### 5.1 Immediate Actions - 0–30 Days

- [ ] Implement MFA for all user types.
- [ ] Deploy a WAF with SQL injection protection rules.
- [ ] Review and restrict database privileges.
- [ ] Implement parameterized queries and input validation.
- [ ] Review authentication and authorization controls.

### 5.2 Short-Term Actions - 30–90 Days

- [ ] Implement a security awareness training program.
- [ ] Deploy database activity monitoring.
- [ ] Conduct a comprehensive RBAC privilege review.
- [ ] Improve API response filtering and data minimization.
- [ ] Strengthen prescription integrity and approval controls.

### 5.3 Medium-Term Actions - 90–180 Days

- [ ] Implement Privileged Access Management (PAM).
- [ ] Deploy user behavior and access anomaly analytics.
- [ ] Establish and test incident response procedures.
- [ ] Improve DDoS protection and traffic monitoring.
- [ ] Strengthen centralized security monitoring and SIEM capabilities.

### 5.4 Control Gaps

The most significant identified control gaps include:

- Missing WAF protection
- Missing database activity monitoring
- Partial MFA coverage
- Missing security awareness training
- Missing least-privilege database controls
- Missing privileged-access monitoring
- Partial RBAC implementation
- Missing access anomaly detection

The detailed control mapping is available in:

[Security Control Mapping](./control-mapping.md)

### 5.5 Remediation Effort

| Remediation Area | Estimated Effort |
|------------------|------------------|
| MFA implementation | Medium |
| WAF deployment | Medium |
| Security awareness training | Low |
| Database privilege review | Low |
| Database activity monitoring | High |
| RBAC improvements | Medium |
| Privileged Access Management | High |
| User behavior analytics | High |
| Incident response implementation | Medium |

The detailed remediation plan is available in:

[Remediation Plan](./remediation-plan.md)

---

# Appendices

## Appendix A - Complete Threat Register

The complete list of 31 identified STRIDE threats is available in:

[STRIDE Threat Register](./stride-threats.md)

---

## Appendix B - Architecture Diagrams

The Solaris Care Connect 360 architecture is documented in:

[Architecture Diagram](../diagrams/architecture.md)

---

## Appendix C - Data Flow Diagrams

Detailed data flows are documented in:

- [Level 0 DFD](../diagrams/dfd-level0.md)
- [Level 1 DFD](../diagrams/dfd-level1.md)

---

## Appendix D - MITRE ATT&CK Mapping

The detailed MITRE ATT&CK mapping is available in:

[MITRE ATT&CK Mapping](./mitre-mapping.md)

---

## Appendix E - Cyber Kill Chain Analysis

The detailed attack-chain analysis is available in:

[Cyber Kill Chain Analysis](./cyber-kill-chain.md)

---

## Appendix F - Risk Assessment

Supporting risk calculations and methodology are available in:

[Risk Assessment](./risk-assessment.md)

---

## Appendix G - Security Control Mapping

The detailed comparison of existing and required security controls is available in:

[Security Control Mapping](./control-mapping.md)

---

## Appendix H - Remediation Plan

The detailed remediation actions, owners, timelines, and success metrics are available in:

[Remediation Plan](./remediation-plan.md)

---

## Review Schedule

This threat model should be reviewed and updated **quarterly** or whenever significant system, architecture, infrastructure, or security changes occur.

---

*End of Threat Model Report*