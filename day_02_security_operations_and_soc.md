# Day 2 — Security Operations & SOC Fundamentals

## Domain 1.0 — Security Operations (33% of exam)

## What Is a SOC?

A **Security Operations Centre (SOC)** is the team (and facility) responsible for monitoring, detecting, investigating, and responding to cybersecurity threats 24/7.

### SOC Analyst Tiers

| Tier | Role | Responsibilities |
|------|------|-----------------|
| Tier 1 | Alert Analyst | Monitor dashboards, triage alerts, basic analysis, escalate |
| Tier 2 | Incident Responder | Deeper investigation, correlation, containment actions |
| Tier 3 | Threat Hunter / SME | Advanced hunting, malware analysis, threat intel |
| Tier 4 | SOC Manager | Operations, metrics, reporting, team leadership |

**CySA+ targets Tier 1–2 analyst skills.**

---

## Key SOC Functions

### 1. Continuous Monitoring
- Watch SIEM dashboards for anomalies and triggered alerts
- Monitor network flows (NetFlow, IPFIX)
- Review endpoint detection alerts (EDR/XDR)
- Analyse authentication and access logs

### 2. Alert Triage
- Determine: True Positive, False Positive, True Negative, False Negative
- **True Positive (TP):** Real attack, correctly detected
- **False Positive (FP):** Benign activity flagged as malicious (alert fatigue)
- **True Negative (TN):** Benign activity, correctly ignored
- **False Negative (FN):** Real attack missed by detection — most dangerous

### 3. Investigation
- Correlate events across multiple log sources
- Build a timeline of attacker activity
- Determine scope and impact

### 4. Response and Escalation
- Contain affected systems
- Escalate to Tier 2/3 as needed
- Document findings in ticketing system

---

## System & Network Architecture Concepts (Exam Objective 1.1)

### Network Segmentation
- **DMZ (Demilitarized Zone):** Public-facing servers isolated from internal network
- **VLAN:** Logical network separation at Layer 2
- **Zero Trust:** Never trust, always verify — no implicit internal trust
- **Microsegmentation:** Fine-grained network isolation per workload/app

### Infrastructure Types
- **On-premises:** Traditional data center, full visibility
- **Cloud (IaaS/PaaS/SaaS):** Shared responsibility model — visibility varies
- **Hybrid:** Mix of on-prem and cloud — complexity for monitoring
- **OT/ICS/SCADA:** Operational technology — often air-gapped, different protocols

### Log Sources the Exam Loves
| Source | What It Tells You |
|--------|-----------------|
| Firewall logs | Allowed/denied connections, ports, IPs |
| DNS logs | Domain queries — C2 detection, data exfiltration |
| Proxy logs | Web traffic, URLs, user agents |
| Authentication logs | Login success/failure, lateral movement |
| DHCP logs | IP-to-hostname mapping at a point in time |
| NetFlow/IPFIX | Who talked to whom, volume, duration |
| Syslog | Generic system events from servers/network devices |
| EDR telemetry | Process execution, file changes, registry, network |
| Web server logs | HTTP requests, response codes, user agents |

---

## Key Security Concepts for the Exam

### Attack Surface
Everything that can be attacked — applications, services, open ports, user accounts, APIs, physical entry points. CySA+ expects you to know how to **reduce the attack surface**.

### Defence in Depth
Multiple layers of security controls so that failure of one does not compromise the whole. Also called "layered security."

### Principle of Least Privilege
Users and systems should only have the minimum access needed to do their job. Critical for limiting blast radius of a compromise.

### Data Classification
| Level | Description | Example |
|-------|-------------|---------|
| Public | Can be shared freely | Marketing materials |
| Internal | Internal use only | Employee policies |
| Confidential | Need-to-know | Customer PII, trade secrets |
| Restricted/Secret | Highly sensitive, strict controls | Credentials, PHI, PCI data |

### Indicators of Compromise (IoC) vs Indicators of Attack (IoA)
- **IoC:** Evidence that a compromise HAS occurred (malware hash, C2 IP, suspicious file)
- **IoA:** Evidence that an attack IS HAPPENING (unusual process spawning cmd.exe, lateral movement behaviour)

---

## SOC Metrics (Exam Objective 4.2)

| Metric | Definition | Why It Matters |
|--------|------------|----------------|
| **MTTD** | Mean Time to Detect | How long before we notice an attack |
| **MTTR** | Mean Time to Respond/Repair | How long to contain + restore |
| **MTTC** | Mean Time to Contain | How long to stop spread |
| **Dwell Time** | Time between compromise and detection | Lower = better |
| **Alert volume** | Number of alerts per day | High = alert fatigue |
| **FP rate** | False positive percentage | High = tuning needed |

---

## Quick Review Questions

1. What is the difference between an IoC and an IoA?
2. A SIEM alerts on a login from an unusual country. An analyst checks and it's the CEO on a business trip. What type of alert is this?
3. Which log source would BEST help identify DNS-based C2 communication?
4. What metric measures how long an attacker was in the network before detection?
5. A user account is used after business hours from a new IP. What should a Tier 1 analyst do first?

**Answers:**
1. IoC = evidence of past compromise; IoA = evidence of active attack in progress
2. False Positive
3. DNS logs
4. Dwell time
5. Triage the alert — check if it's expected/authorised travel, escalate if suspicious
