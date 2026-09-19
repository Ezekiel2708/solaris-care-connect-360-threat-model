# Attack Trees - Solaris Care Connect 360

## Goal: Steal Patient Records

```mermaid
flowchart TD
    Goal[Steal Patient Records]

    Goal --> External[External Attack]
    Goal --> Insider[Insider Attack]
    Goal --> Physical[Physical Access]

    External --> SQLi[SQL Injection]
    External --> Phish[Phishing]
    External --> API[API Exploitation]

    SQLi --> FindVuln[Find SQLi Vulnerability]
    SQLi --> Exploit[Exploit to Dump Data]
    SQLi --> Exfil[Exfiltrate Data]

    Phish --> Recon[Identify Targets]
    Phish --> Craft[Craft Phishing Email]
    Phish --> Creds[Steal Credentials]
    Phish --> Access[Access System]

    Insider --> Abuse[Abuse Privileges]
    Insider --> Escalate[Escalate Privileges]
    Insider --> Export[Export Data]

    Physical --> Break[Break into Facility]
    Physical --> Access2[Access Workstation]
    Physical --> Steal[Steal Data]
```

## Easiest Path Analysis

| Path | Steps | Difficulty | Detection Risk |
|------|-------|------------|----------------|
| SQL Injection | 3 | Medium | Low (if no WAF) |
| Phishing | 4 | Low | Medium |
| Insider | 3 | Low | High (if monitored) |
| Physical | 3 | High | High |

**Most Likely Attack**: Phishing (low difficulty, medium detection)
**Highest Impact**: SQL Injection (can dump entire database)

## Mitigation Priority

Focus controls on the easiest paths:
1. Security awareness training (blocks phishing)
2. WAF and input validation (blocks SQLi)
3. Privileged access monitoring (detects insider)
