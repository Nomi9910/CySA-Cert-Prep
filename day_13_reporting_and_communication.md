# Day 13 — Reporting & Communication

## Domain 4.0 — Reporting and Communication (17% of exam)

## Why Reporting Matters

A security analyst who finds critical vulnerabilities or stops an attack but fails to communicate it clearly has only done half the job. Domain 4.0 tests your ability to produce and interpret security reports for different audiences.

---

## Vulnerability Management Reporting (Objective 4.1)

### Report Types by Audience

| Audience | Report Type | Content Focus |
|----------|-------------|---------------|
| **Executive / C-Suite** | Executive Summary | Risk posture, trends, business impact, cost |
| **IT Management** | Operations Report | Assets affected, patch compliance, SLA status |
| **System Owners** | Technical Report | Specific CVEs, affected systems, remediation steps |
| **Board** | Risk Dashboard | Risk scores, regulatory compliance status, KPIs |
| **Auditors** | Compliance Report | Controls status, evidence, exceptions |

### Executive Summary Structure
```
1. OVERVIEW (1 paragraph)
   - High-level summary of scan coverage
   - Overall risk posture change from last period

2. KEY FINDINGS
   - Critical: X (↑3 from last month)
   - High: X
   - Medium: X
   - Top 3 vulnerabilities requiring immediate attention

3. TRENDING
   - MTTR trending graph (improving or worsening?)
   - Vulnerability density over time
   - Patch compliance rate: 94% (target: 98%)

4. RISK EXPOSURE
   - Highest risk systems
   - Regulatory risk (PCI, HIPAA compliance gaps)

5. RECOMMENDATIONS
   - Immediate actions needed
   - Resource requirements
   - Strategic improvements

6. METRICS DASHBOARD
   - Scorecard of KPIs vs targets
```

---

## Key Security Metrics (Must Know for Exam)

| Metric | Formula / Definition | Target |
|--------|---------------------|--------|
| **MTTD** (Mean Time to Detect) | Avg time from compromise to discovery | Lower is better |
| **MTTR** (Mean Time to Respond/Repair) | Avg time from detection to resolution | Lower is better |
| **MTTC** (Mean Time to Contain) | Avg time to stop spread after detection | Lower is better |
| **Dwell time** | Time attacker present before detection | Industry avg: 21 days |
| **Patch compliance rate** | % systems patched within SLA | >95% typical target |
| **Vulnerability density** | Total vulns / total assets | Trending down = improving |
| **False positive rate** | FP alerts / total alerts × 100 | Lower is better |
| **Alert fatigue indicator** | Alerts analysts can't investigate | Drives SIEM tuning |
| **Risk reduction rate** | % risk score reduction per period | Positive trend required |
| **Recurrence rate** | Previously fixed vulns that reappear | Should be near zero |

---

## Incident Response Reporting (Objective 4.2)

### Initial Incident Report (Immediate)
```
INCIDENT REPORT — INITIAL NOTIFICATION
────────────────────────────────────────
Incident ID:      IR-2025-0047
Severity:         Critical
Date/Time:        2025-06-25 14:32 UTC
Reported By:      John Smith, Tier 2 Analyst
Classification:   Ransomware Infection

SUMMARY:
Ransomware detected on 3 servers in Finance department.
Active encryption observed. Systems isolated at 14:35 UTC.

AFFECTED SYSTEMS:
- fin-server-01 (10.0.5.10)
- fin-server-02 (10.0.5.11)  
- fin-workstation-07 (10.0.5.87)

CURRENT STATUS:
- Systems isolated from network
- IR team engaged
- Forensic acquisition in progress

IMMEDIATE ACTIONS TAKEN:
14:32 - Alert received from EDR
14:33 - Tier 2 analyst confirmed TP
14:35 - Isolated 3 affected systems
14:40 - IR team notified
14:45 - Management notified

NEXT UPDATE: 16:00 UTC
POINT OF CONTACT: ir-team@company.com | +1-800-555-0100
```

### After-Action / Lessons Learned Report
```
POST-INCIDENT REPORT
────────────────────────────────────────
Incident ID:    IR-2025-0047
Date Closed:    2025-06-27
Severity:       Critical
Type:           Ransomware (LockBit 3.0)

TIMELINE:
2025-06-20 09:14 — Initial compromise via phishing email
2025-06-25 14:32 — Detection by EDR (5.3-day dwell time)
2025-06-25 14:35 — Containment (3 minutes from detection)
2025-06-27 09:00 — Systems restored from backup

ROOT CAUSE:
Phishing email with malicious Excel attachment.
User credentials captured by keylogger.
Attacker used credentials for 5 days before deploying ransomware.

IMPACT:
- 3 servers encrypted, 2 workstations
- 3 hours of finance operations disrupted
- No data confirmed exfiltrated (network logs reviewed)
- Estimated cost: $45,000 (recovery, staff overtime)

WHAT WORKED:
✔ EDR detected ransomware quickly (3-minute containment)
✔ Clean backups available and restorable
✔ IR plan followed correctly
✔ Management communication was timely

WHAT DIDN'T WORK:
✗ Phishing email bypassed email filter
✗ 5-day dwell time — credential abuse not detected
✗ Finance team not trained on phishing (recent arrival)

LESSONS LEARNED:
1. Implement UEBA/behaviour analytics for credential anomalies
2. Mandatory phishing awareness training for all new starters
3. Update email filter with new IOCs
4. Enable MFA for all privileged accounts (not just domain admins)

ACTION ITEMS:
Owner         | Action                           | Due Date
─────────────────────────────────────────────────────────
Security Team | Deploy UEBA for credential abuse | 30 days
IT Team       | Enable MFA on all admin accounts | 7 days
HR            | Mandatory security training      | 14 days
Email Team    | Update phishing filter rules     | 3 days
```

---

## Communication Considerations

### Internal Communication
- **Who to notify:** Depends on severity (IR policy defines escalation)
- **What to say:** Facts only — avoid speculation until investigation confirms
- **When:** Immediately for Critical/High; within hours for Medium
- **Format:** Verbal for urgency + written for documentation

### External Communication
| Situation | Who to Notify |
|-----------|--------------|
| Personal data breach | Affected individuals, data protection authority (GDPR: 72 hours) |
| Payment card data | Card brands (Visa, Mastercard), acquiring bank, PCI SSC |
| Healthcare data | HHS OCR (HIPAA: 60 days, or 30 days for vendor breach) |
| Publicly traded company | SEC (material cybersecurity incidents) |
| Government contractors | US-CERT/CISA, contracting agency |
| Law enforcement | FBI Cyber Division, Secret Service |

### Regulatory Notification Timelines
| Regulation | Notification Deadline |
|------------|----------------------|
| **GDPR** | 72 hours to supervisory authority |
| **HIPAA** | 60 days of discovery (30 for business associates) |
| **PCI DSS** | Immediately to card brands and acquirer |
| **CCPA** | "In the most expedient time possible" |
| **NY SHIELD** | "In the most expedient time possible" |

---

## Stakeholder Communication Tips for the Exam

- **Executive** audience → Business impact, cost, risk, trends (NOT technical details)
- **Technical** audience → CVE details, affected versions, specific remediation steps
- **Legal** audience → Facts, timeline, scope, regulatory obligations
- **Avoid** → Speculation, premature conclusions, blame, incomplete information

---

## Practice Questions

1. A CISO asks for a vulnerability management update. What format is most appropriate?
2. GDPR requires notification to a supervisory authority within how many hours of discovering a data breach?
3. What metric measures the average time between when an attacker compromises a system and when it's detected?
4. A lessons learned report identifies that the same vulnerability reappeared 3 months after being patched. Which metric tracks this?
5. When communicating an incident to executives, what should you prioritise?

**Answers:**
1. Executive summary dashboard — focus on risk posture, trends, and business impact, not technical CVE details
2. 72 hours (GDPR Article 33)
3. Dwell time (also called "breakout time" in some contexts)
4. Recurrence rate — tracks vulnerabilities that return after remediation
5. Business impact, financial exposure, regulatory risk, and recommendations — NOT technical details like CVE numbers or CVSS scores
