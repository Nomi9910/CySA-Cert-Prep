# Day 15 — Practice Questions & Exam Strategies

## Final Exam Prep

## Exam-Taking Strategies

### Question Strategy
```
1. READ the entire question before looking at answers
2. IDENTIFY the key constraint words:
   "MOST likely" → best fit among plausible answers
   "BEST" → optimal, not just correct
   "FIRST" → sequence matters (triage, volatility, etc.)
   "LEAST" → opposite thinking required
   "EXCEPT" → find the odd one out

3. ELIMINATE obviously wrong answers (usually 2 clear eliminates)
4. COMPARE the remaining 2 with the scenario context
5. PICK the answer that best fits the SCENARIO, not just general knowledge
```

### Common CySA+ Trap Patterns
- **Two correct answers** — one is more specific/appropriate to the scenario
- **Technically correct but wrong context** — valid security action but not for THIS scenario
- **Out of order** — technically valid but wrong sequence (e.g., patching before containing)
- **Scope creep** — answer goes beyond what's asked

### Time Management
- 165 minutes / 85 questions ≈ 1 min 56 sec per question
- **Strategy:** 
  - Pass on PBQs (performance-based) initially
  - Answer all multiple choice first (~60 minutes)
  - Return to PBQs with remaining time
  - Flag uncertain answers and review if time permits
  - Never leave questions unanswered — no penalty for guessing

---

## 50 Practice Questions (Full Domain Coverage)

### Domain 1.0 — Security Operations

**1.** A SIEM alert shows 47 failed login attempts from a single IP in 2 minutes, followed by a successful login. What should the analyst investigate FIRST?
- A) Block the source IP in the firewall
- B) Determine if the successful login is legitimate
- C) Reset the user's password immediately
- D) Escalate to management

**Answer: B** — Always investigate to understand the situation before taking action. The successful login may be legitimate (business travel) or malicious.

---

**2.** DNS logs show queries for `bXVzaWM.freeupdates.top` every 30 seconds. The domain was registered 3 days ago. What is the MOST likely explanation?
- A) Legitimate software update check
- B) DNS cache poisoning
- C) DNS tunnelling for C2 communication
- D) BGP route hijacking

**Answer: C** — Long encoded subdomain labels at regular intervals to a new domain strongly indicates DNS tunnelling / C2 beaconing.

---

**3.** An analyst notices `powershell.exe` spawned by `excel.exe` in the process tree. What technique does this MOST likely represent?
- A) Pass-the-hash
- B) Macro malware using LOLBin technique
- C) Scheduled task execution
- D) Remote code execution via SMB

**Answer: B** — Excel spawning PowerShell is a classic macro malware indicator using Living-off-the-Land binaries.

---

**4.** A SOC analyst receives 3,000 alerts per day but can only investigate 200. What should the analyst recommend FIRST?
- A) Hire more analysts
- B) Implement automated response for all alerts
- C) Tune SIEM rules to reduce false positives
- D) Disable low-priority alerts permanently

**Answer: C** — SIEM tuning reduces noise while maintaining detection capability. Hiring more staff or disabling alerts are secondary/risky approaches.

---

**5.** Which log source would BEST help an analyst identify lateral movement after initial compromise?
- A) DNS logs
- B) Web server access logs
- C) Authentication logs combined with NetFlow data
- D) Endpoint antivirus logs

**Answer: C** — Lateral movement involves moving between systems. Authentication logs show account usage across systems; NetFlow shows connections between internal hosts.

---

**6.** An EDR alert shows a process with no corresponding file on disk making outbound connections on port 443. What type of malware is MOST likely?
- A) Ransomware
- B) Rootkit
- C) Fileless malware
- D) Bootkit

**Answer: C** — Fileless malware executes only in memory with no on-disk file artifact.

---

**7.** Which MITRE ATT&CK tactic describes an attacker maintaining access after initial compromise?
- A) Initial Access
- B) Execution
- C) Persistence
- D) Privilege Escalation

**Answer: C** — Persistence covers techniques used to maintain foothold (scheduled tasks, registry run keys, services).

---

**8.** A threat hunter hypothesises that an attacker is using T1059.001 (PowerShell). What data source should they query?
- A) Firewall logs
- B) DNS logs
- C) Windows PowerShell Script Block logs (Event ID 4104)
- D) NetFlow data

**Answer: C** — PowerShell Script Block logging (Event ID 4104) captures PowerShell commands executed, enabling hunting for malicious scripts.

---

### Domain 2.0 — Vulnerability Management

**9.** A vulnerability scan shows CVSS 9.8 on a development server with no internet exposure and no production data. A CVSS 7.2 vulnerability exists on an internet-facing e-commerce server. Which should be prioritised?
- A) The CVSS 9.8 — always patch the highest score first
- B) The CVSS 7.2 — context makes it higher real-world risk
- C) Both simultaneously — they are equal priority
- D) Neither — wait for the next scheduled maintenance window

**Answer: B** — CVSS alone is insufficient; context (internet-facing, production data) elevates real-world risk above raw score.

---

**10.** What does a CVSS vector of AV:N/AC:L/PR:N/UI:N represent?
- A) Low-risk vulnerability requiring local access
- B) Critical vulnerability exploitable by authenticated users
- C) Network-exploitable vulnerability requiring no privileges or user interaction
- D) Vulnerability requiring complex attack conditions

**Answer: C** — AV:N = network, AC:L = low complexity, PR:N = no privileges, UI:N = no user interaction. Maximum exploitability.

---

**11.** An analyst validates a scanner finding and determines the software version is not actually vulnerable — the banner displayed an old version. What should they do?
- A) Report the finding as confirmed
- B) Mark it as a false positive and document the reasoning
- C) Escalate to management
- D) Re-scan the system

**Answer: B** — False positives should be documented and suppressed with clear justification to maintain accurate risk data.

---

**12.** Which standard provides a catalogue of actively exploited vulnerabilities that federal agencies must prioritise?
- A) NVD
- B) OVAL
- C) CISA KEV
- D) CVSS

**Answer: C** — The CISA Known Exploited Vulnerabilities catalogue lists CVEs being actively exploited; agencies must remediate them within mandated timeframes.

---

**13.** A critical vulnerability cannot be patched for 45 days due to application compatibility. What is the BEST immediate mitigation?
- A) Accept the risk
- B) Implement a WAF rule or IPS signature as a compensating control
- C) Shut down the affected system
- D) Increase monitoring only

**Answer: B** — Compensating controls (virtual patching via WAF/IPS) provide immediate risk reduction while awaiting the formal patch.

---

**14.** What does EPSS provide that CVSS does not?
- A) Impact assessment
- B) Probability that a vulnerability will be exploited in the next 30 days
- C) Asset criticality scoring
- D) Regulatory compliance mapping

**Answer: B** — EPSS (Exploit Prediction Scoring System) predicts exploitation likelihood; CVSS measures theoretical severity.

---

### Domain 3.0 — Incident Response

**15.** During a ransomware incident, an analyst is told to immediately shut down the affected server to stop encryption. What should they do FIRST?
- A) Shut down immediately as instructed
- B) Capture a memory dump and network connections before shutdown
- C) Contact legal counsel
- D) Restore from backup immediately

**Answer: B** — Memory contains volatile evidence (encryption keys, running processes, network connections) lost on shutdown. Capture first.

---

**16.** Which phase of the NIST IR lifecycle involves verifying no backdoors remain and resetting all compromised credentials?
- A) Containment
- B) Detection
- C) Eradication
- D) Recovery

**Answer: C** — Eradication removes all traces of the attack, including backdoors, attacker accounts, and persistence mechanisms.

---

**17.** An attacker had access to a network for 18 days before detection. What metric does this represent?
- A) MTTD
- B) MTTR
- C) Dwell time
- D) MTTC

**Answer: C** — Dwell time is the period between initial compromise and detection. MTTD is Mean Time to Detect (from alert generation to analyst confirmation).

---

**18.** A Cyber Kill Chain stage where the attacker's malware connects to an external server for instructions corresponds to which MITRE ATT&CK tactic?
- A) Persistence
- B) Lateral Movement
- C) Command & Control
- D) Exfiltration

**Answer: C** — Stage 6 of the Kill Chain (C2) maps directly to the ATT&CK Command & Control tactic.

---

**19.** What is the purpose of a tabletop exercise in incident response?
- A) Deploy intrusion detection systems
- B) Practise response procedures in a simulated scenario without real systems
- C) Test backup restoration procedures
- D) Audit log retention settings

**Answer: B** — Tabletop exercises are discussion-based simulations where teams walk through hypothetical incidents to identify gaps in plans and procedures.

---

**20.** Before collecting a disk image as forensic evidence, what is REQUIRED to ensure admissibility?
- A) Shut down the system first
- B) Connect a write blocker to prevent modification of the evidence
- C) Get written permission from the user
- D) Create a RAID backup first

**Answer: B** — A write blocker prevents any data from being written to the evidence drive during acquisition, preserving integrity.

---

### Domain 4.0 — Reporting & Communication

**21.** A CISO asks for a vulnerability management status report. What format is MOST appropriate?
- A) Raw scanner output with all CVE details
- B) Executive summary with risk trends, top vulnerabilities, and remediation status
- C) Detailed technical remediation steps for each finding
- D) Network diagram showing all affected systems

**Answer: B** — Executive audience requires business-focused information: trends, risk posture, priorities — not raw technical data.

---

**22.** Under GDPR, organisations must report personal data breaches to a supervisory authority within:
- A) 24 hours
- B) 48 hours
- C) 72 hours
- D) 7 days

**Answer: C** — GDPR Article 33 requires notification within 72 hours of becoming aware of the breach.

---

**23.** Which metric best indicates whether a vulnerability management programme is improving over time?
- A) Total number of vulnerabilities found
- B) Mean Time to Remediate trending downward
- C) Number of scans performed per month
- D) Total CVSS score of all findings

**Answer: B** — MTTR trending down shows the programme is remediating faster. Total vuln count can rise legitimately (better scanning coverage).

---

**24.** A post-incident report reveals the same SQL injection vulnerability was patched 4 months ago but reappeared. Which metric tracks this?
- A) MTTD
- B) Vulnerability density
- C) Recurrence rate
- D) False positive rate

**Answer: C** — Recurrence rate tracks vulnerabilities that return after remediation, indicating patch or process failures.

---

**25.** When communicating an incident to the board, what is the MOST important information to include?
- A) Technical details of the CVE exploited
- B) Business impact, regulatory exposure, and recommended actions
- C) CVSS scores of all vulnerabilities
- D) Full incident timeline with every technical action

**Answer: B** — Board-level communication focuses on business impact, liability, and strategic decisions — not technical details.

---

## Quick-Reference Key Terms

| Term | One-Line Definition |
|------|---------------------|
| SIEM | Centralises logs, correlates events, generates alerts |
| EDR | Endpoint visibility: processes, files, network, behaviour |
| XDR | Cross-layer detection: endpoint + network + cloud + email |
| SOAR | Security Orchestration, Automation, and Response |
| TTP | Tactics, Techniques, and Procedures (attacker fingerprint) |
| IoC | Indicator of Compromise (evidence attack occurred) |
| IoA | Indicator of Attack (evidence attack is in progress) |
| CVSS | Severity score 0-10 for vulnerabilities |
| EPSS | Probability % a CVE will be exploited in next 30 days |
| CISA KEV | Actively exploited CVEs list — patch immediately |
| CTI | Cyber Threat Intelligence — evidence-based knowledge |
| STIX/TAXII | Standard format/protocol for sharing threat intel |
| ATT&CK | MITRE framework of adversary tactics and techniques |
| Kill Chain | 7-stage model of a targeted attack |
| Diamond Model | Adversary-Capability-Infrastructure-Victim analysis |
| NIST IR | 4-phase IR: Prepare → Detect → Contain/Eradicate/Recover → Post-incident |
| PICERL | IR mnemonic: Prepare, Identify, Contain, Eradicate, Recover, Lessons |
| Chain of Custody | Document trail proving evidence integrity |
| Order of Volatility | Collect evidence most-volatile first (RAM → disk) |
| GDPR | EU privacy law — 72hr breach notification |
| HIPAA | US healthcare privacy — 60-day breach notification |
| PCI DSS | Payment card security standard — 12 requirements |
| Zero Trust | Never trust, always verify — no implicit internal trust |
| Shared Responsibility | Cloud: provider secures infrastructure; you secure data/config |
| CASB | Cloud Access Security Broker — cloud app visibility |
| CSPM | Cloud Security Posture Management — config compliance |

---

## Final Exam Checklist

```
BEFORE EXAM DAY:
□ Review all 5 domains — no gaps
□ Memorise NIST IR phases and what happens in each
□ Memorise CVSS metric meanings (AV, AC, PR, UI, S, C, I, A)
□ Know Kill Chain stages in order
□ Know top MITRE ATT&CK tactics and key techniques
□ Know log source → attack mapping
□ Know regulatory timelines (GDPR 72hr, HIPAA 60 days)
□ Know order of volatility for forensics
□ Know security control types (preventive, detective, corrective, compensating)
□ Practice CVSS vector interpretation

ON EXAM DAY:
□ Read questions carefully — identify "BEST", "FIRST", "MOST"
□ Skip PBQs initially, do multiple choice first
□ Eliminate two wrong answers, compare remaining two to scenario
□ Never leave questions unanswered
□ Flag uncertain questions and review
□ Trust preparation — don't second-guess too much

GOOD LUCK! You've got this. 🎯
```

---

## Additional Study Resources

- **Official CompTIA CySA+ Study Guide** (Mike Chapple, David Seidl)
- **Professor Messer's CySA+ Course** (free video series at professormesser.com)
- **CompTIA CySA+ Practice Exams** (Jason Dion)
- **MITRE ATT&CK Navigator** — navigator.attack.mitre.org
- **NIST SP 800-61r2** — Computer Security Incident Handling Guide (free PDF)
- **CVSS Calculator** — nvd.nist.gov/vuln-metrics/cvss/v3-calculator
- **CISA KEV Catalogue** — cisa.gov/known-exploited-vulnerabilities-catalog
- **TryHackMe / HackTheBox** — Hands-on blue team labs
- **Splunk Boss of the SOC (BOTS)** — Free SIEM analysis challenges
