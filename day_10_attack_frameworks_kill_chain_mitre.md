# Day 10 — Attack Frameworks: Kill Chain & MITRE ATT&CK

## Domain 3.0 — Incident Response and Management (Objective 3.1)

## Why Attack Frameworks Matter for CySA+

Attack frameworks give security analysts a structured way to:
- Understand the **stages** of an attack
- Map defensive controls to attacker **techniques**
- Communicate about threats using **common language**
- Identify **detection gaps** in security coverage
- Prioritise **where to detect and respond**

---

## Lockheed Martin Cyber Kill Chain

The Kill Chain (2011) models a targeted attack as 7 sequential stages. Disrupting ANY stage stops the attack.

```
┌────────────────────────────────────────────────────────────────┐
│  CYBER KILL CHAIN                                              │
│                                                                │
│  1 → RECONNAISSANCE → Information gathering                   │
│  2 → WEAPONISATION  → Create exploit/payload                  │
│  3 → DELIVERY       → Transmit to victim                      │
│  4 → EXPLOITATION   → Trigger vulnerability                   │
│  5 → INSTALLATION   → Malware installed, foothold             │
│  6 → C2 (Command & Control) → Attacker gains control          │
│  7 → ACTIONS ON OBJECTIVES → Goal achieved                    │
└────────────────────────────────────────────────────────────────┘
```

### Kill Chain Stages — Detailed

| Stage | Attacker Activity | Defender Action |
|-------|------------------|-----------------|
| **1. Reconnaissance** | OSINT, LinkedIn, Shodan, DNS enumeration | Monitor for scanning, review public exposure |
| **2. Weaponisation** | Create malicious doc, build exploit | Threat intelligence, malware analysis |
| **3. Delivery** | Phishing email, drive-by download, USB | Email filtering, web proxy, AV |
| **4. Exploitation** | Exploit vulnerability, macro execution | Patching, EDR, application control |
| **5. Installation** | Dropper installs backdoor, persistence | EDR, file integrity monitoring |
| **6. C2** | Malware calls home, attacker gets shell | Network monitoring, DNS filtering, IPS |
| **7. Actions** | Data theft, ransomware, sabotage | DLP, data segmentation, backups |

### Key Kill Chain Concept: "Left of Boom"
- "Left" = earlier in the kill chain
- "Boom" = the attacker achieving their goal (Stage 7)
- Better to stop attacks in Stages 1-3 (before execution)
- Many organisations only detect at Stage 5-6 — too late

---

## MITRE ATT&CK Framework (Reviewed from Day 6 + expanded)

### ATT&CK vs Kill Chain Comparison

| Aspect | Kill Chain | MITRE ATT&CK |
|--------|-----------|--------------|
| Age | 2011 | 2013 (ongoing) |
| Focus | Attack stages | Specific techniques |
| Detail level | High-level 7 stages | 14 tactics, 200+ techniques, 400+ sub-techniques |
| Use case | Strategic planning | Operational detection |
| Evidence | Military model | Real-world attack data |
| Updates | Static | Continuously updated |

### Mapping Kill Chain → ATT&CK Tactics

| Kill Chain | ATT&CK Tactics |
|-----------|----------------|
| Reconnaissance | Reconnaissance |
| Weaponisation | Resource Development |
| Delivery | Initial Access |
| Exploitation | Execution, Privilege Escalation |
| Installation | Persistence, Defence Evasion |
| C2 | Command & Control |
| Actions on Objectives | Collection, Exfiltration, Impact |

### Using ATT&CK for Detection Coverage

```
# ATT&CK Navigator — visualise detection coverage
# https://mitre-attack.github.io/attack-navigator/

# Colour code your coverage:
Red     → No detection capability
Yellow  → Partial detection
Green   → Full detection capability

# Example gap analysis:
T1059.001 PowerShell        → Yellow (logs exist, no alerting)
T1055     Process Injection → Red    (no EDR deployed)
T1078     Valid Accounts    → Green  (UEBA detects anomalies)
```

---

## Diamond Model of Intrusion Analysis

Complements Kill Chain and ATT&CK — analyses intrusions through 4 features:

```
         Adversary
        /         \
       /           \
Infrastructure — Victim
       \           /
        \         /
        Capability
```

| Feature | Description |
|---------|-------------|
| **Adversary** | Who is attacking (threat actor, APT group) |
| **Capability** | What tools/techniques they use (malware, exploits) |
| **Infrastructure** | What they use to attack (C2 servers, domains, IPs) |
| **Victim** | Who is being targeted |

**Meta-features:** Timestamp, phase (kill chain stage), result, direction, methodology

**Why analysts use it:** Link different attacks to the same adversary by correlating infrastructure and capability patterns.

---

## NIST Cybersecurity Framework (CSF)

Five core functions — not just for incident response, but for overall security posture:

```
IDENTIFY → Understand assets, risks, and current security posture
PROTECT  → Implement safeguards (access control, training, patches)
DETECT   → Monitor for cybersecurity events (SIEM, IDS, EDR)
RESPOND  → Take action on detected incidents
RECOVER  → Restore capabilities after an incident
```

**CySA+ maps to DETECT and RESPOND primarily.**

---

## Threat Modelling

Systematic process for identifying threats to a system:

### STRIDE (Microsoft Threat Model)
| Letter | Threat | Example |
|--------|--------|---------|
| **S** | Spoofing | IP spoofing, email spoofing |
| **T** | Tampering | Modifying data in transit |
| **R** | Repudiation | Denying actions occurred |
| **I** | Information Disclosure | Data breach |
| **D** | Denial of Service | DDoS |
| **E** | Elevation of Privilege | Privilege escalation |

### PASTA (Process for Attack Simulation and Threat Analysis)
7-stage risk-centric threat modelling methodology focused on business impact.

---

## Practice Questions

1. An attacker sends a phishing email with a malicious Word document. Which Kill Chain stage does this represent?
2. After infection, the malware connects to a C2 server every 60 seconds. Which Kill Chain stage is this?
3. What is the main advantage of MITRE ATT&CK over the Kill Chain?
4. An analyst maps detected techniques to the ATT&CK Navigator and finds a red area at T1055 (Process Injection). What should they do?
5. Using the Diamond Model, an analyst links three separate incidents because they all use the same C2 infrastructure. What Diamond Model feature enables this?

**Answers:**
1. Delivery (Stage 3) — the phishing email delivers the malicious payload
2. Command & Control (Stage 6) — the malware communicates with the attacker's infrastructure
3. ATT&CK provides specific, named techniques with real-world examples; Kill Chain only gives broad stages — ATT&CK is much more actionable for detection engineering
4. Deploy an EDR solution that can detect process injection behaviour to fill the detection gap
5. Infrastructure — shared infrastructure links disparate attacks to the same adversary
