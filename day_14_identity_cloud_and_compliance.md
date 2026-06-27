# Day 14 — Identity Security, Cloud Security & Compliance

## Cross-Domain Topics — All Domains

## Identity and Access Management (IAM)

### Authentication vs Authorisation
- **Authentication (AuthN):** Who are you? (verify identity)
- **Authorisation (AuthZ):** What are you allowed to do? (access control)
- **Accountability:** What did you do? (audit logs, non-repudiation)

### Authentication Factors
| Factor | Type | Examples |
|--------|------|---------|
| Something you know | Knowledge | Password, PIN, security question |
| Something you have | Possession | Smart card, hardware token (YubiKey), OTP app |
| Something you are | Inherence | Fingerprint, retina, face recognition |
| Somewhere you are | Location | IP range, GPS location |
| Something you do | Behaviour | Keystroke dynamics, gait analysis |

**MFA** = 2+ factors from different categories

### Identity Attack Techniques

| Attack | Description | Defence |
|--------|-------------|---------|
| **Credential stuffing** | Using breached username/password lists | MFA, rate limiting, haveibeenpwned integration |
| **Password spraying** | Common passwords against many accounts | Account lockout, MFA, long passphrases |
| **Brute force** | Trying all combinations | Account lockout, CAPTCHAs |
| **Pass-the-Hash** | Replay NTLM hash without knowing password | Disable NTLM, use Kerberos, credential guard |
| **Pass-the-Ticket** | Replay Kerberos ticket | Short ticket lifetimes, monitoring |
| **Kerberoasting** | Request service tickets, crack offline | Long random service account passwords, gMSAs |
| **Golden Ticket** | Forge Kerberos TGT using krbtgt hash | Protect krbtgt, detect unusual TGTs |
| **Silver Ticket** | Forge Kerberos service ticket | Service account hardening |
| **SAML injection** | Manipulate SAML assertions | Sign and validate SAML, strict attribute validation |

### Privileged Account Security
```
PAM (Privileged Access Management) controls:
□ Just-In-Time (JIT) access — elevate only when needed
□ Just-Enough Privilege (JEP) — minimal required permissions
□ Session recording — video/keystroke capture of admin sessions
□ Password vaulting — auto-rotation, no human sees passwords
□ Dual approval — require second approval for critical actions

Tools: CyberArk, BeyondTrust, Delinea (Thycotic)
```

---

## Cloud Security (Critical for CS0-003)

### Shared Responsibility Model
```
CLOUD PROVIDER RESPONSIBLE FOR:
  ✔ Physical data centres
  ✔ Network infrastructure
  ✔ Hypervisor
  ✔ Managed service platforms

CUSTOMER RESPONSIBLE FOR:
  ✔ Data encryption
  ✔ Identity and access management
  ✔ Operating system (IaaS only)
  ✔ Applications
  ✔ Security configurations
  ✔ Network controls (security groups, NACLs)
```

### Cloud Security Concepts

| Concept | Description |
|---------|-------------|
| **CASB** | Cloud Access Security Broker — visibility into cloud app usage |
| **CSPM** | Cloud Security Posture Management — continuous config compliance |
| **CWPP** | Cloud Workload Protection Platform — runtime protection |
| **CNAPP** | Cloud-Native App Protection Platform — combines CSPM + CWPP |
| **SSPM** | SaaS Security Posture Management — SaaS config monitoring |

### Cloud-Specific Threats
```
Misconfigured S3 buckets → Publicly exposed sensitive data
Over-permissive IAM roles → Privilege escalation, lateral movement
Exposed metadata endpoint → SSRF attack steals cloud credentials
  AWS: http://169.254.169.254/latest/meta-data/iam/security-credentials/
Unsecured APIs → Unauthorised access
Missing encryption → Data exposure at rest
Supply chain attacks → Compromised container images, dependencies
Cryptojacking → Stolen compute resources for crypto mining
```

### Cloud Monitoring Tools
```bash
# AWS:
AWS CloudTrail        # API call logging (authentication, resource changes)
AWS GuardDuty         # ML-based threat detection
AWS Security Hub      # Centralised security findings
AWS Config            # Configuration compliance monitoring
VPC Flow Logs         # Network traffic logging (like NetFlow)

# Azure:
Microsoft Defender for Cloud  # Security posture + threat protection
Azure Monitor / Sentinel      # SIEM + threat intelligence
Azure Policy                  # Compliance enforcement

# GCP:
Security Command Center       # Asset inventory + threat detection
Cloud Audit Logs              # API call logging
Cloud Armour                  # DDoS and WAF protection
```

---

## Compliance Frameworks (Must Know for Exam)

| Framework | Sector | Key Requirement |
|-----------|--------|----------------|
| **PCI DSS** | Payment cards | 12 requirements, quarterly scans, penetration testing |
| **HIPAA** | Healthcare (US) | PHI protection, breach notification, risk assessment |
| **SOC 2** | Service orgs | Trust Service Criteria (Security, Availability, Integrity) |
| **GDPR** | EU data privacy | Data subject rights, 72-hr breach notification, DPO |
| **NIST CSF** | Critical infrastructure | Identify-Protect-Detect-Respond-Recover |
| **ISO 27001** | All sectors | Information security management system (ISMS) |
| **CMMC** | US defense contractors | Cybersecurity maturity model, 3 levels |
| **FISMA** | US federal agencies | NIST SP 800-53 controls, ATO process |
| **CIS Controls** | All sectors | 18 prioritised controls, implementation groups |

### CIS Controls — Top 6 (Most Impactful)
```
CIS Control 1: Inventory and Control of Enterprise Assets
CIS Control 2: Inventory and Control of Software Assets
CIS Control 3: Data Protection
CIS Control 4: Secure Configuration of Enterprise Assets
CIS Control 5: Account Management
CIS Control 6: Access Control Management
```

---

## Security Architecture Concepts

### Zero Trust Architecture
```
Core Principles:
"Never trust, always verify"
"Assume breach"
"Verify explicitly"

Components:
- Micro-segmentation
- MFA for all access
- Device compliance verification
- Least-privilege access
- Continuous monitoring and validation
```

### Defence in Depth
```
Layer 1: Physical security
Layer 2: Network perimeter (firewall, IPS)
Layer 3: Network segmentation (VLANs, microsegmentation)
Layer 4: Endpoint protection (EDR, AV, patch management)
Layer 5: Application security (WAF, SAST, DAST)
Layer 6: Data security (encryption, DLP, access controls)
Layer 7: User education and awareness
```

### Security Control Types

| Category | Examples |
|----------|---------|
| **Preventive** | Firewall, MFA, encryption, access control |
| **Detective** | SIEM, IDS, audit logs, EDR alerts |
| **Corrective** | Incident response, patch deployment, restore from backup |
| **Deterrent** | Warning banners, visible cameras, security policies |
| **Compensating** | WAF when patch unavailable, network restriction |
| **Physical** | Locks, badges, cameras, guards |
| **Administrative** | Policies, procedures, training, background checks |
| **Technical** | Software/hardware controls (firewalls, encryption) |

---

## Practice Questions

1. A cloud scan finds an S3 bucket with public-read ACL containing employee PII. Who is responsible for this misconfiguration?
2. An attacker steals an NTLM hash and uses it to authenticate without cracking it. What attack is this?
3. What tool provides continuous visibility into cloud security posture and misconfigurations?
4. Which compliance regulation requires healthcare organisations to report breaches within 60 days?
5. A WAF blocks an exploit when a vulnerability cannot be patched immediately. What type of control is this?

**Answers:**
1. The customer — IAM and data access configuration is always the customer's responsibility in the shared responsibility model
2. Pass-the-Hash attack — using the hash directly for NTLM authentication
3. CSPM (Cloud Security Posture Management) — continuously monitors cloud configurations for compliance violations
4. HIPAA (Health Insurance Portability and Accountability Act)
5. Compensating control — provides alternative protection when the primary control (patch) is not available
