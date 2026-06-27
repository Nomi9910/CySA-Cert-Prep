# Day 11 — Incident Response Lifecycle (NIST/PICERL)

## Domain 3.0 — Incident Response and Management (Objective 3.2, 3.3)

## What Is an Incident?

A cybersecurity **incident** is a violation or imminent threat of violation of computer security policies, acceptable use policies, or standard security practices.

**Events vs Incidents:**
- **Event:** Any observable occurrence (login, file access, network connection)
- **Incident:** An event with negative security impact (data breach, malware infection, DoS)

---

## NIST Incident Response Lifecycle

NIST SP 800-61r2 defines the standard IR lifecycle — critical knowledge for CySA+:

```
┌─────────────────────────────────────────────────────────┐
│              NIST IR LIFECYCLE                          │
│                                                         │
│  1. PREPARATION                                         │
│       ↓                                                 │
│  2. DETECTION & ANALYSIS                                │
│       ↓                                                 │
│  3. CONTAINMENT, ERADICATION & RECOVERY                 │
│       ↓                                                 │
│  4. POST-INCIDENT ACTIVITY                              │
│       ↑_____________________________________________↑   │
│  (Feeds back into Preparation for next incident)        │
└─────────────────────────────────────────────────────────┘
```

### PICERL (Common Alternative Mnemonic)
**P**reparation → **I**dentification → **C**ontainment → **E**radication → **R**ecovery → **L**essons Learned

---

## Phase 1: Preparation

Building the capability to respond effectively BEFORE incidents occur.

### Key Activities
```
□ Incident Response Plan (IRP) — documented procedures
□ Incident Response Team (IRT) — roles and responsibilities
□ Communication plan — who to call, escalation path
□ SIEM, IDS, EDR deployment — detection tools
□ Playbooks — step-by-step runbooks for common incidents
□ Tabletop exercises — practise scenarios with stakeholders
□ Forensic toolkit — prepared and ready
□ Legal and law enforcement contacts
□ Evidence handling procedures
□ Backup systems verified and tested
□ Jump bag — portable forensics kit
```

### Incident Response Team Roles
| Role | Responsibility |
|------|---------------|
| IR Lead / Manager | Coordinate response, decision authority |
| Forensic Analyst | Evidence collection and analysis |
| Threat Hunter | Deep technical analysis |
| Legal Counsel | Legal advice, law enforcement liaison |
| Communications | Internal and external messaging |
| HR | Insider threat cases, employee matters |
| IT Operations | System restoration, network changes |
| Executive Sponsor | Business decisions, major incidents |

---

## Phase 2: Detection & Analysis (Identification)

Determining that an incident has occurred and understanding its scope.

### Detection Sources
- SIEM alerts
- EDR/antivirus alerts
- User reports
- Help desk tickets
- Threat intelligence feeds
- External notification (law enforcement, CERT, business partner)
- Automated threat detection

### Analysis Steps
```
1. INITIAL TRIAGE
   - Is this a true positive or false positive?
   - What type of incident is this?
   - What systems are affected?

2. SCOPE DETERMINATION
   - How many systems/users affected?
   - What data may be at risk?
   - Is the attacker still active?

3. PRIORITY CLASSIFICATION
   Critical → High → Medium → Low
   Based on: data sensitivity, business impact, spread

4. TIMELINE CONSTRUCTION
   - When did it start? (Patient zero)
   - What happened, in what order?
   - What is the attacker's current position?

5. DOCUMENTATION
   Start incident ticket/log immediately
   Every action, finding, and decision documented with timestamp
```

### Incident Classification
| Category | Examples |
|----------|---------|
| Malware | Ransomware, RAT, botnet infection |
| Unauthorised access | Compromised credentials, insider threat |
| Denial of Service | DDoS, resource exhaustion |
| Data breach | PII/PHI/PCI exfiltration |
| Web application attack | SQLi, defacement, web shell |
| Physical | Theft, unauthorised physical access |
| Supply chain | Compromised software update |

---

## Phase 3: Containment, Eradication & Recovery

### Containment
Stop the spread while preserving evidence for investigation.

```
SHORT-TERM CONTAINMENT (immediate):
- Isolate affected hosts (network quarantine)
- Block malicious IPs/domains in firewall
- Disable compromised accounts
- Block malicious file hashes in EDR
- Take system snapshot/memory dump BEFORE changes

LONG-TERM CONTAINMENT (extended):
- Patch or rebuild systems
- Implement additional monitoring
- Temporary firewall rules
- Segment affected network segment
```

**Key principle:** Contain WITHOUT destroying evidence. Memory should be captured before shutdown.

### Eradication
Remove the threat from the environment entirely.

```
□ Remove malware from all affected systems
□ Remove attacker-created accounts
□ Remove persistence mechanisms:
  - Scheduled tasks
  - Registry run keys
  - Services
  - Startup folders
  - Cron jobs (Linux)
□ Remove web shells from web servers
□ Verify no backdoors remain
□ Patch vulnerabilities used for initial access
□ Reset compromised credentials (ALL affected accounts)
□ Rotate API keys, service account passwords
```

### Recovery
Restore systems to normal operation safely.

```
□ Restore from clean backups (verify backup integrity first)
□ Rebuild from gold image if severely compromised
□ Apply all patches before bringing back online
□ Verify system integrity before restoration
□ Re-enable accounts with new credentials
□ Monitor rebuilt systems closely (48-72 hours)
□ Validate business operations are restored
□ Document restoration steps
```

---

## Phase 4: Post-Incident Activity (Lessons Learned)

Conducted within 2 weeks of incident resolution.

### Lessons Learned Meeting
Attendees: IR team, IT, management (as appropriate)

**Questions to answer:**
1. What happened, and how did we find out?
2. What was the timeline?
3. What worked well?
4. What did not work well?
5. What would we do differently?
6. What detection/prevention gaps were exposed?
7. What policy/procedure changes are needed?
8. What training is needed?

### Outputs
- Written lessons learned report
- Updated IR plan and playbooks
- New/updated SIEM detection rules
- Patch/vulnerability management improvements
- Updated CLAUDE.md / runbooks

---

## Incident Response Plan (IRP) Components

```
An IRP must include:
□ Purpose and scope
□ Roles and responsibilities
□ Incident classification criteria
□ Detection and reporting procedures
□ Escalation procedures
□ Containment procedures (per incident type)
□ Eradication procedures
□ Recovery procedures
□ Communication plan (internal, external, legal)
□ Evidence handling and chain of custody
□ Contact list (team, law enforcement, regulators, vendors)
□ Retention schedule for incident data
```

---

## Practice Questions

1. During an incident, an analyst wants to shut down a compromised server immediately. What should they do FIRST?
2. What is the difference between containment and eradication?
3. After recovering from a ransomware attack, what is the MOST important post-incident activity?
4. A user reports their laptop is acting strangely. An analyst confirms malware. What phase of NIST IR are we in?
5. Which document provides step-by-step procedures for responding to a specific type of incident?

**Answers:**
1. Capture a memory dump and system snapshot BEFORE shutting down — volatile evidence (running processes, network connections, encryption keys) is lost on shutdown
2. Containment = stop the spread, preserve evidence; Eradication = completely remove the threat from all systems
3. Conduct a lessons learned review/meeting to identify gaps and prevent recurrence
4. Detection & Analysis (Phase 2) — incident confirmed and being analysed
5. Playbook / Runbook — detailed step-by-step IR procedures for a specific incident type
