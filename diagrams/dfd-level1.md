# Level 1 Data Flow Diagram

```mermaid
flowchart TB
    subgraph External
        Patient[Patient]
        Doctor[Doctor]
    end

    subgraph "1.0 Authentication"
        Login[1.1 Login]
        MFA[1.2 MFA Verify]
        Session[1.3 Session Mgmt]
    end

    subgraph "2.0 Patient Records"
        View[2.1 View Records]
        Update[2.2 Update Records]
        Audit[2.3 Audit Log]
    end

    subgraph "3.0 Prescriptions"
        Create[3.1 Create Rx]
        Validate[3.2 Validate]
        Send[3.3 Send to Pharmacy]
    end

    subgraph Data
        UserDB[(User Store)]
        PatientDB[(Patient Records)]
        AuditDB[(Audit Logs)]
    end

    Patient -->|Credentials| Login
    Login -->|Verify| UserDB
    Login -->|Token| MFA
    MFA -->|Session| Session

    Doctor -->|View Request| View
    View -->|Query| PatientDB
    View -->|Log| Audit
    Audit -->|Store| AuditDB

    Doctor -->|New Rx| Create
    Create -->|Check| Validate
    Validate -->|Approved| Send
```
## Data Classification

The following table identifies the sensitivity of the primary data processed within Solaris Care Connect 360 and the minimum security controls required to protect each data type.

| Data Type             | Classification | Encryption Required                           | Logging Required                           |
| --------------------- | -------------- | --------------------------------------------- | ------------------------------------------ |
| Patient Records       | PHI – Critical | Yes (AES-256 at rest, TLS 1.2+ in transit)    | Yes – Full audit logging                   |
| Login Credentials     | PII – High     | Yes (bcrypt password hashing, TLS in transit) | Yes – Authentication and access logs       |
| Session Tokens        | Sensitive      | Yes (Signed JWT, HTTPS Only, Secure Cookies)  | Yes – Session creation and validation logs |
| Audit Logs            | Internal       | Yes (AES-256 at rest)                         | N/A                                        |
| Lab Results           | PHI – Critical | Yes (AES-256, TLS 1.2+)                       | Yes – Full audit logging                   |
| Prescription Data     | PHI – Critical | Yes (AES-256, TLS 1.2+)                       | Yes – Full audit logging                   |
| Insurance Information | PII – High     | Yes (AES-256, TLS 1.2+)                       | Yes – Access and transaction logs          |
| Patient Demographics  | PII – High     | Yes (AES-256, TLS 1.2+)                       | Yes – Access logging                       |

### Key Security Considerations

* **Protected Health Information (PHI)** requires the highest level of protection due to regulatory requirements such as HIPAA.
* All PHI and PII must be encrypted both **in transit** and **at rest**.
* Authentication events, data access, and administrative actions must be logged to support auditing, compliance, and incident investigations.
* Session tokens must be cryptographically signed and transmitted only over secure HTTPS connections.
* Audit logs must be protected from unauthorized modification and retained according to organizational and regulatory policies.
