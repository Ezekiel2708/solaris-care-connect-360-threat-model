# Remediation Plan - Solaris Care Connect 360

**Version:** 1.0  
**Date:** 18 September 2026  
**Classification:** Confidential

---

## 1. Purpose

This remediation plan defines the security actions required to reduce the risks identified during the threat modeling and risk assessment of Solaris Care Connect 360.

The plan is based on the eight risks formally assessed in the risk register:

- I1 - SQL Injection / Data Breach
- S1 - Credential Phishing
- E3 - SQL Injection to DBA Access
- T2 - Prescription Tampering
- E1 - Patient-to-Doctor Privilege Escalation
- I2 - Excessive Data Exposure
- D1 - DDoS Attack
- R1 - Prescription Approval Denial

Remediation priorities are based on the calculated likelihood × impact risk scores.

---

## 2. Remediation Priority

| Priority | Risk Score | Required Action |
|---|---:|---|
| Critical | 16-25 | Immediate remediation |
| High | 10-15 | Remediate as a priority |
| Medium | 5-9 | Remediate within planned security improvement cycle |
| Low | 1-4 | Monitor or address through routine security activities |

The current assessment contains:

- **2 Critical risks**
- **4 High risks**
- **2 Medium risks**
- **0 Low risks**

---

# 3. Critical Risk Remediation

## 3.1 I1 - SQL Injection / Data Breach

**Risk Score:** 20  
**Severity:** Critical  
**Likelihood:** 4 - Likely  
**Impact:** 5 - Severe  

### Risk

An attacker may exploit SQL injection vulnerabilities in the Patient Records Service or other application components to access, modify, or extract sensitive patient information.

### Remediation Actions

1. Replace dynamically constructed SQL queries with parameterized queries or prepared statements.
2. Implement strict server-side input validation.
3. Review all API endpoints that interact with PostgreSQL.
4. Conduct secure code reviews focused on injection vulnerabilities.
5. Deploy a Web Application Firewall (WAF) to provide an additional layer of protection.
6. Perform authenticated penetration testing against application and API endpoints.
7. Implement database activity monitoring.
8. Ensure application database accounts have only the permissions required by the application.

### Owner

**Development Team / Database Administration / Security Team**

### Priority

**Immediate**

### Target

**0-30 days**

### Success Metrics

- Zero critical SQL injection vulnerabilities identified during security testing.
- All database queries use parameterized queries or approved safe query mechanisms.
- WAF rules are deployed and tested.
- Database application accounts follow least-privilege principles.
- Database activity monitoring generates alerts for suspicious queries.

---

## 3.2 S1 - Credential Phishing

**Risk Score:** 16  
**Severity:** Critical  
**Likelihood:** 4 - Likely  
**Impact:** 4 - Major  

### Risk

Attackers may obtain patient, doctor, administrator, or employee credentials through phishing or other credential theft techniques.

### Remediation Actions

1. Enforce multi-factor authentication (MFA) for all privileged accounts.
2. Extend MFA coverage to all users where supported by the platform.
3. Use phishing-resistant authentication for privileged and administrative access where practical.
4. Strengthen email filtering and phishing detection.
5. Implement suspicious-login detection.
6. Provide regular security awareness and phishing training.
7. Monitor authentication logs for unusual login locations, devices, and patterns.
8. Establish an account compromise response procedure.

### Owner

**Security Operations / IAM Team**

### Priority

**Immediate**

### Target

**0-30 days**

### Success Metrics

- 100% of privileged accounts protected by MFA.
- MFA coverage expanded across supported user accounts.
- Phishing simulation results improve over successive exercises.
- Suspicious authentication events generate alerts.
- Compromised-account response procedures are documented and tested.

---

# 4. High Risk Remediation

## 4.1 E3 - SQL Injection to DBA Access

**Risk Score:** 15  
**Severity:** High  
**Likelihood:** 3 - Possible  
**Impact:** 5 - Severe  

### Risk

A successful SQL injection attack could potentially be used to obtain elevated database privileges and increase the attacker's access to sensitive systems and data.

### Remediation Actions

1. Review all PostgreSQL roles and permissions.
2. Remove unnecessary administrative privileges from application accounts.
3. Separate application database accounts from administrative database accounts.
4. Restrict direct database access to authorized systems and personnel.
5. Harden PostgreSQL configuration.
6. Monitor privileged database activity.
7. Review database permissions periodically.
8. Investigate abnormal database privilege changes.

### Owner

**Database Administration / Security Team**

### Priority

**High**

### Target

**0-30 days**

### Success Metrics

- No application account has unnecessary DBA privileges.
- Privileged database accounts are documented.
- Database access is restricted to authorized sources.
- Privileged database activity is monitored.
- Database privilege reviews are completed regularly.

---

## 4.2 T2 - Prescription Tampering

**Risk Score:** 10  
**Severity:** High  
**Likelihood:** 2 - Unlikely  
**Impact:** 5 - Severe  

### Risk

Unauthorized modification of prescriptions could alter medication names, dosages, or instructions and potentially cause harm to patients.

### Remediation Actions

1. Implement strong role-based authorization for prescription creation and modification.
2. Restrict prescription changes to authorized healthcare personnel.
3. Require appropriate approval workflows for high-risk prescription changes.
4. Implement integrity validation for prescription records.
5. Maintain detailed audit logs for prescription creation and modification.
6. Record the identity, timestamp, and action associated with each prescription change.
7. Alert on unusual prescription modifications.
8. Test authorization controls for horizontal and vertical privilege escalation.

### Owner

**Application Development / IAM / Security Team**

### Priority

**High**

### Target

**31-90 days**

### Success Metrics

- Unauthorized users cannot create or modify prescriptions.
- Prescription modifications are fully audited.
- High-risk prescription changes follow defined approval workflows.
- Authorization testing produces no critical findings.
- Suspicious prescription changes generate alerts.

---

## 4.3 E1 - Patient-to-Doctor Privilege Escalation

**Risk Score:** 12  
**Severity:** High  
**Likelihood:** 3 - Possible  
**Impact:** 4 - Major  

### Risk

A patient account could potentially obtain privileges intended for doctors or other higher-privileged users.

### Remediation Actions

1. Implement and enforce role-based access control (RBAC).
2. Separate patient, doctor, and administrator roles.
3. Apply least-privilege principles to application permissions.
4. Review role-assignment workflows.
5. Require strong authorization checks on every sensitive API endpoint.
6. Conduct authorization and IDOR testing.
7. Review privileged-role assignments regularly.
8. Alert on unexpected role changes.

### Owner

**IAM Team / Application Development**

### Priority

**High**

### Target

**31-90 days**

### Success Metrics

- Patient accounts cannot access doctor or administrator functionality.
- All sensitive API endpoints enforce server-side authorization.
- Privileged role assignments are reviewed regularly.
- Authorization testing finds no critical privilege escalation vulnerabilities.
- Unexpected privilege changes generate security alerts.

---

## 4.4 I2 - Excessive Data Exposure

**Risk Score:** 12  
**Severity:** High  
**Likelihood:** 4 - Likely  
**Impact:** 3 - Moderate  

### Risk

APIs may return more patient or application data than is necessary for a legitimate operation.

### Remediation Actions

1. Review API responses for unnecessary fields.
2. Apply data minimization to API responses.
3. Implement field-level authorization where appropriate.
4. Remove sensitive information from unnecessary API responses.
5. Review mobile and web API endpoints.
6. Implement automated API security testing.
7. Monitor API access patterns for unusual data retrieval.
8. Review bulk-data access and export functionality.

### Owner

**API Development Team / Security Team**

### Priority

**High**

### Target

**31-90 days**

### Success Metrics

- API responses contain only data required for the requested operation.
- Sensitive fields are protected by authorization controls.
- API security tests identify no critical excessive-data-exposure issues.
- Bulk data access is logged and monitored.

---

# 5. Medium Risk Remediation

## 5.1 D1 - DDoS Attack

**Risk Score:** 9  
**Severity:** Medium  
**Likelihood:** 3 - Possible  
**Impact:** 3 - Moderate  

### Risk

A distributed denial-of-service attack could overwhelm the API Gateway or supporting infrastructure and prevent legitimate users from accessing the platform.

### Remediation Actions

1. Implement API rate limiting.
2. Configure request throttling for abusive clients.
3. Deploy appropriate DDoS protection at the network or cloud layer.
4. Monitor API traffic volumes.
5. Configure autoscaling where appropriate.
6. Establish traffic anomaly alerts.
7. Document DDoS response procedures.
8. Test the organization's ability to respond to service-disruption events.

### Owner

**Infrastructure / DevOps / Security Operations**

### Priority

**Medium**

### Target

**31-90 days**

### Success Metrics

- Rate limiting is enabled on critical APIs.
- Abnormal traffic generates alerts.
- DDoS protection is configured and tested.
- DDoS response procedures are documented.
- Service availability is monitored continuously.

---

## 5.2 R1 - Prescription Approval Denial

**Risk Score:** 6  
**Severity:** Medium  
**Likelihood:** 2 - Unlikely  
**Impact:** 3 - Moderate  

### Risk

A doctor or authorized user may later deny having approved or issued a prescription if sufficient evidence of the transaction is not maintained.

### Remediation Actions

1. Maintain detailed audit logs for prescription approvals.
2. Record the authenticated identity of the approving user.
3. Record timestamps using synchronized system clocks.
4. Record relevant transaction identifiers.
5. Protect audit logs from unauthorized modification.
6. Restrict access to audit records.
7. Monitor unusual changes to prescription records.
8. Periodically test audit-log completeness.

### Owner

**Security Operations / Application Development**

### Priority

**Medium**

### Target

**31-90 days**

### Success Metrics

- Prescription approvals produce complete audit records.
- Each approval is associated with an authenticated user.
- Audit timestamps are synchronized.
- Audit records cannot be modified by ordinary application users.
- Audit-log completeness is tested periodically.

---

# 6. Remediation Roadmap

## Phase 1 - Immediate: 0-30 Days

Focus on the two Critical risks and the most important supporting controls.

| Action | Risk(s) | Owner | Priority |
|---|---|---|---|
| Parameterized SQL queries | I1, E3 | Development | Critical |
| Server-side input validation | I1 | Development | Critical |
| WAF deployment | I1 | Infrastructure/Security | Critical |
| MFA for privileged accounts | S1 | IAM/Security | Critical |
| Phishing protection | S1 | Security Operations | Critical |
| Database privilege review | E3 | Database Administration | High |
| Database activity monitoring | I1, E3 | Security/DBA | High |

---

## Phase 2 - Short Term: 31-90 Days

Focus on authorization, prescription integrity, API security, and service availability.

| Action | Risk(s) | Owner | Priority |
|---|---|---|---|
| RBAC review and improvement | E1, T2 | IAM/Application | High |
| API authorization testing | E1, I2 | Security/API Team | High |
| API data-minimization review | I2 | API Development | High |
| Prescription integrity controls | T2 | Application Development | High |
| Prescription audit improvements | T2, R1 | Security/Application | High |
| Rate limiting | D1 | Infrastructure | Medium |
| DDoS protection | D1 | Infrastructure/Security | Medium |
| Audit-log protection | R1 | Security Operations | Medium |

---

## Phase 3 - Medium Term: 91-180 Days

Focus on strengthening detection, monitoring, and long-term security maturity.

| Action | Risk(s) | Owner | Priority |
|---|---|---|---|
| Advanced authentication monitoring | S1 | Security Operations | Medium |
| Access anomaly detection | E1 | IAM/Security | Medium |
| Database monitoring improvements | I1, E3 | Security/DBA | Medium |
| API security automation | I2 | DevSecOps | Medium |
| Security monitoring integration | All | Security Operations | Medium |
| DDoS response exercises | D1 | Infrastructure/Security | Medium |
| Authorization penetration testing | E1, T2, I2 | Security Team | Medium |

---

# 7. Ongoing Security Activities

The following activities should continue after the initial remediation phases.

### Vulnerability Management

- Perform regular vulnerability assessments.
- Patch operating systems, applications, dependencies, and infrastructure.
- Prioritize vulnerabilities based on risk.
- Track remediation through a vulnerability management process.

### Penetration Testing

Conduct periodic penetration testing covering:

- Web applications
- APIs
- Authentication
- Authorization
- Mobile applications
- Database security
- External attack surface
- Third-party integrations

### Secure Software Development

Integrate security into the software development lifecycle through:

- Secure coding standards
- Code review
- Static Application Security Testing (SAST)
- Dynamic Application Security Testing (DAST)
- Dependency scanning
- Secret scanning
- API security testing

### Identity and Access Management

- Review user permissions regularly.
- Remove inactive accounts.
- Review privileged accounts.
- Enforce least privilege.
- Monitor unusual privilege changes.

### Logging and Monitoring

Monitor:

- Authentication events
- Privileged actions
- Patient-record access
- Prescription changes
- Database activity
- API anomalies
- Administrative actions
- Security alerts

### Backup and Recovery

- Maintain secure backups.
- Restrict backup access.
- Test restoration procedures.
- Monitor backup integrity.
- Maintain disaster-recovery procedures.

---

# 8. Remediation Success Criteria

The remediation program will be considered effective when:

1. All Critical risks have documented remediation and verification.
2. Critical SQL injection vulnerabilities are eliminated.
3. Privileged accounts are protected with MFA.
4. Application database accounts follow least-privilege principles.
5. Sensitive API responses have been reviewed and minimized.
6. Patient, doctor, and administrator permissions are properly separated.
7. Prescription modifications are strongly authorized and audited.
8. Security monitoring detects suspicious authentication, database, and application activity.
9. DDoS protection and rate limiting are operational for critical services.
10. Security testing confirms that implemented controls are effective.

---

# 9. Verification and Validation

Remediation should not be considered complete merely because a control has been deployed.

Each remediation action should be verified through appropriate testing.

Examples:

| Control | Verification Method |
|---|---|
| SQL injection prevention | SAST, DAST, penetration testing |
| WAF | Controlled security testing and rule validation |
| MFA | Authentication configuration review |
| RBAC | Authorization and privilege-escalation testing |
| Database least privilege | Database permission audit |
| API data minimization | API response testing |
| Prescription integrity | Application and authorization testing |
| Audit logging | Log completeness testing |
| DDoS protection | Configuration review and controlled testing |
| Security awareness | Training completion and phishing simulations |

---

# 10. Risk Acceptance and Exceptions

Any risk that cannot be remediated within the target timeframe should be formally documented.

A risk exception should include:

- Risk ID
- Reason for exception
- Business justification
- Existing compensating controls
- Residual risk
- Risk owner
- Approval authority
- Expiration/review date

Risk exceptions should be reviewed periodically and should not be treated as permanent replacements for remediation.

---

# 11. Review Schedule

The remediation plan should be reviewed:

- Monthly during active remediation.
- Quarterly after the initial remediation program.
- After significant architecture changes.
- After major security incidents.
- After major changes to authentication, authorization, or data processing.
- During future threat-model reviews.

---

# 12. Conclusion

The remediation strategy prioritizes the risks with the highest calculated likelihood and impact while addressing the underlying security weaknesses that contribute to multiple threats.

Immediate attention should be given to SQL injection/data-breach risk and credential phishing. Database privilege management, authorization controls, prescription integrity, API data minimization, DDoS protection, and auditability should then be strengthened through the subsequent remediation phases.

Remediation effectiveness should be demonstrated through security testing, monitoring, configuration reviews, and periodic reassessment rather than relying solely on control deployment.