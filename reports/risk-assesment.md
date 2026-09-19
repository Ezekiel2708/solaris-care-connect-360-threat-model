# Risk Assessment - Solaris Care Connect 360

## 1. Risk Assessment Methodology

The Solaris Care Connect 360 threat model uses a qualitative risk assessment methodology based on the relationship between **Likelihood** and **Impact**.

Risk is calculated using:

**Risk Score = Likelihood × Impact**

Each identified risk is assigned:

- A **Likelihood score** from 1 to 5
- An **Impact score** from 1 to 5
- A resulting **Risk Score** from 1 to 25
- A corresponding severity level

The scores represent project assessment assumptions and should be reviewed and validated through security testing, operational data, and subject-matter expertise.

---

## 2. Likelihood Rating

| Score | Rating | Description |
|------:|--------|-------------|
| **1** | Rare | The threat is unlikely to occur under normal conditions. |
| **2** | Unlikely | The threat could occur but is not expected frequently. |
| **3** | Possible | The threat could reasonably occur. |
| **4** | Likely | The threat is expected to occur if appropriate controls are not implemented. |
| **5** | Almost Certain | The threat is highly likely to occur without effective controls. |

---

## 3. Impact Rating

| Score | Rating | Description |
|------:|--------|-------------|
| **1** | Negligible | Minimal effect on the system or users. |
| **2** | Minor | Limited operational or security impact. |
| **3** | Moderate | Noticeable impact requiring corrective action. |
| **4** | Major | Significant impact to operations, data, or users. |
| **5** | Severe | Serious compromise of confidentiality, integrity, availability, or patient safety. |

---

## 4. Risk Severity Classification

| Risk Score | Severity | Response |
|-----------:|----------|----------|
| **16–25** | Critical | Immediate remediation required |
| **10–15** | High | Remediation should be prioritized |
| **5–9** | Medium | Address through planned remediation |
| **1–4** | Low | Monitor or address as resources permit |

---

## 5. Risk Matrix

| Likelihood ↓ / Impact → | 1 | 2 | 3 | 4 | 5 |
|-------------------------|---:|---:|---:|---:|---:|
| **5 - Almost Certain** | 5 M | 10 H | 15 H | 20 C | 25 C |
| **4 - Likely** | 4 L | 8 M | 12 H | 16 C | 20 C |
| **3 - Possible** | 3 L | 6 M | 9 M | 12 H | 15 H |
| **2 - Unlikely** | 2 L | 4 L | 6 M | 8 M | 10 H |
| **1 - Rare** | 1 L | 2 L | 3 L | 4 L | 5 M |

**Legend:**  
L = Low  
M = Medium  
H = High  
C = Critical

---

## 6. Assessed Risks

The following eight risks were selected for detailed risk assessment based on their relevance to the Solaris Care Connect 360 architecture and identified STRIDE threats.

| Risk ID | Risk | Likelihood | Impact | Score | Severity |
|---------|------|-----------:|-------:|------:|----------|
| **I1** | SQL Injection - Data Breach | 4 | 5 | **20** | **Critical** |
| **S1** | Credential Phishing | 4 | 4 | **16** | **Critical** |
| **E3** | SQL Injection → DBA Access | 3 | 5 | **15** | **High** |
| **T2** | Prescription Tampering | 2 | 5 | **10** | **High** |
| **E1** | Patient → Doctor Privilege Escalation | 3 | 4 | **12** | **High** |
| **I2** | Excessive Data Return | 4 | 3 | **12** | **High** |
| **D1** | DDoS Attack | 3 | 3 | **9** | **Medium** |
| **R1** | Prescription Approval Denial | 2 | 3 | **6** | **Medium** |

---

## 7. Individual Risk Assessments

### I1 - SQL Injection / Data Breach

**Likelihood:** 4 - Likely  
**Impact:** 5 - Severe  
**Risk Score:** 4 × 5 = **20**  
**Severity:** **Critical**

An SQL injection vulnerability in the Patient Records Service or another database-connected application component could allow an attacker to access, modify, or extract sensitive patient information.

The impact is severe because the platform manages approximately 50,000 patient records.

**Existing Control Status:**

- Prepared statements and input validation: **Partial**
- Web Application Firewall: **Missing**
- Database activity monitoring: **Missing**

**Recommended Controls:**

- Parameterized queries
- Input validation
- Web Application Firewall
- Secure code review
- Database activity monitoring
- Regular application security testing

---

### S1 - Credential Phishing

**Likelihood:** 4 - Likely  
**Impact:** 4 - Major  
**Risk Score:** 4 × 4 = **16**  
**Severity:** **Critical**

Phishing could result in the compromise of patient, doctor, or administrative credentials. Compromised credentials could subsequently be used to access sensitive information or perform unauthorized actions.

**Existing Control Status:**

- Multi-factor authentication: **Partial**
- Security awareness training: **Missing**
- Email security gateway: **Implemented**

**Recommended Controls:**

- MFA for all appropriate user accounts
- Phishing-resistant authentication for privileged users
- Security awareness training
- Email security filtering
- Suspicious-login detection
- Session and token revocation

---

### E3 - SQL Injection Leading to DBA Access

**Likelihood:** 3 - Possible  
**Impact:** 5 - Severe  
**Risk Score:** 3 × 5 = **15**  
**Severity:** **High**

An attacker exploiting SQL injection could potentially obtain elevated database access if application accounts have excessive privileges.

This could increase the impact of a successful application-layer compromise.

**Existing Control Status:**

- Least-privilege database accounts: **Missing**
- Privileged access monitoring: **Missing**

**Recommended Controls:**

- Least-privilege database accounts
- Separate application and administrative database accounts
- Database privilege reviews
- Database activity monitoring
- Privileged-access monitoring
- Network segmentation

---

### T2 - Prescription Tampering

**Likelihood:** 2 - Unlikely  
**Impact:** 5 - Severe  
**Risk Score:** 2 × 5 = **10**  
**Severity:** **High**

Unauthorized modification of prescription information could affect medication names, dosages, or other prescription details.

The potential impact is severe because prescription integrity can directly affect patient care.

**Recommended Controls:**

- Strong role-based authorization
- Prescription approval workflows
- Integrity validation
- Audit logging
- Digital signatures for critical transactions
- Monitoring of prescription modifications

---

### E1 - Patient → Doctor Privilege Escalation

**Likelihood:** 3 - Possible  
**Impact:** 4 - Major  
**Risk Score:** 3 × 4 = **12**  
**Severity:** **High**

A vulnerability in authorization or role management could allow a patient account to obtain privileges intended for a doctor.

This could expose medical records or allow unauthorized prescription and patient-management functions.

**Existing Control Status:**

- Role-Based Access Control: **Partial**
- Access anomaly detection: **Missing**

**Recommended Controls:**

- Strong RBAC
- Least privilege
- Role-assignment restrictions
- Privilege-change logging
- Access anomaly detection
- Regular access reviews

---

### I2 - Excessive Data Return

**Likelihood:** 4 - Likely  
**Impact:** 3 - Moderate  
**Risk Score:** 4 × 3 = **12**  
**Severity:** **High**

Poorly designed API responses could expose more patient information than required by the requesting application or user.

This could increase the amount of sensitive information exposed if an account or API endpoint is compromised.

**Recommended Controls:**

- Data minimization
- API response filtering
- Field-level authorization
- Least-privilege access
- API security testing
- Monitoring for unusual data access

---

### D1 - DDoS Attack

**Likelihood:** 3 - Possible  
**Impact:** 3 - Moderate  
**Risk Score:** 3 × 3 = **9**  
**Severity:** **Medium**

A Distributed Denial-of-Service attack could overwhelm the API Gateway or supporting infrastructure and prevent legitimate users from accessing the platform.

**Recommended Controls:**

- Rate limiting
- DDoS protection
- Web Application Firewall
- Traffic monitoring
- Autoscaling
- Network filtering
- Availability monitoring

---

### R1 - Prescription Approval Denial

**Likelihood:** 2 - Unlikely  
**Impact:** 3 - Moderate  
**Risk Score:** 2 × 3 = **6**  
**Severity:** **Medium**

A doctor or other authorized user could later dispute having approved or issued a prescription if transaction records do not provide sufficient evidence.

**Recommended Controls:**

- Immutable audit logging
- Synchronized timestamps
- Strong user authentication
- Digital signatures for critical transactions
- Detailed prescription audit trails

---

## 8. Risk Distribution

The eight assessed risks are distributed as follows:

| Severity | Number of Risks | Percentage |
|----------|----------------:|-----------:|
| **Critical** | 2 | 25% |
| **High** | 4 | 50% |
| **Medium** | 2 | 25% |
| **Low** | 0 | 0% |
| **Total** | **8** | **100%** |

---

## 9. Risk Treatment Priorities

The risk assessment indicates that remediation should focus on:

### Critical Risks

**I1 - SQL Injection / Data Breach**

Priority controls:
- Parameterized queries
- Input validation
- WAF
- Database activity monitoring

**S1 - Credential Phishing**

Priority controls:
- MFA
- Phishing-resistant authentication
- Security awareness training
- Email security

### High Risks

**E3 - SQL Injection → DBA Access**

Priority controls:
- Least-privilege database accounts
- Database privilege review
- Privileged-access monitoring

**T2 - Prescription Tampering**

Priority controls:
- RBAC
- Prescription approval workflows
- Integrity controls
- Audit logging

**E1 - Privilege Escalation**

Priority controls:
- RBAC
- Least privilege
- Access reviews
- Anomaly detection

**I2 - Excessive Data Return**

Priority controls:
- API response filtering
- Data minimization
- Field-level authorization

### Medium Risks

**D1 - DDoS**

Priority controls:
- Rate limiting
- DDoS protection
- Traffic monitoring

**R1 - Prescription Approval Denial**

Priority controls:
- Immutable audit logging
- Synchronized timestamps
- Transaction integrity controls

---

## 10. Risk Assessment Limitations

This assessment is based on the defined Solaris Care Connect 360 architecture and threat-modeling assumptions.

The risk scores are **qualitative project estimates** rather than measured probabilities. Actual risk levels should be reassessed using:

- Penetration testing results
- Vulnerability assessment findings
- Security monitoring data
- Incident history
- Application testing
- Database security assessments
- Changes to the system architecture
- Changes to the threat environment

Risk ratings should therefore be reviewed whenever significant changes are made to the platform and as part of the scheduled threat-model review.

---

## 11. Conclusion

The assessment identified **8 prioritized risks**, consisting of **2 Critical, 4 High, and 2 Medium risks**.

The most significant areas requiring remediation are SQL injection protection, credential security, database privilege management, authorization, and protection of sensitive API responses.

The results of this assessment should be used together with the security control mapping and remediation plan to determine the controls required to reduce the identified risks.

---

*Risk scores in this document represent project assessment assumptions and should be validated through appropriate security testing and operational evidence.*