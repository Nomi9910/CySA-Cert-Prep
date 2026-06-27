# Day 6 — Threat Intelligence & Threat Hunting

## Domain 1.0 — Security Operations (Objectives 1.4, 1.5)

## Threat Intelligence (CTI)

**Cyber Threat Intelligence (CTI)** is evidence-based knowledge about threats — including context, mechanisms, indicators, implications, and actionable advice — used to make decisions about responding to threats.

### Intelligence Types by Level

| Level | Name | Audience | Content |
|-------|------|----------|---------|
| **Strategic** | Big picture | C-suite, board | Nation-state threats, industry trends, geopolitical risk |
| **Operational** | Campaign-level | SOC managers | Threat actor TTPs, attack campaigns, targeting |
| **Tactical** | IoC-level | SOC analysts | IP addresses, domains, file hashes, signatures |
| **Technical** | Tool-level | Threat hunters | Malware samples, exploits, vulnerability details |

### Intelligence Sources

**Open-Source (OSINT):**
- **MITRE ATT&CK** — Adversary TTPs framework (free)
- **VirusTotal** — File/URL/IP reputation
- **AbuseIPDB** — IP reputation database
- **Shodan** — Internet-exposed device search engine
- **CIRCL MISP** — Threat sharing platform
- **AlienVault OTX** — Open threat exchange
- **CISA Alerts** — US government threat advisories
- **NVD/CVE** — Vulnerability database

**Commercial:**
- Recorded Future, Mandiant Advantage, CrowdStrike Intelligence, Palo Alto Unit 42

**ISACs (Information Sharing and Analysis Centers):**
- Industry-specific threat sharing (FS-ISAC for finance, H-ISAC for healthcare, etc.)

### Threat Intelligence Platforms (TIP)
- Aggregate and normalise intelligence from multiple feeds
- Automate IoC enrichment and SIEM integration
- Examples: MISP, ThreatConnect, Anomali STAXX, ThreatQuotient

### TAXII and STIX
- **STIX (Structured Threat Information eXpression):** Standard language for describing CTI
- **TAXII (Trusted Automated eXchange of Intelligence Information):** Standard protocol for sharing STIX data
- Together: STIX/TAXII enables automated CTI sharing between organisations

---

## MITRE ATT&CK Framework (Critical for Exam)

ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge) is a globally-accessible knowledge base of adversary TTPs based on real-world observations.

### ATT&CK Structure
```
Tactic         → The WHY (goal of the attacker)
  Technique    → The HOW (method used)
    Sub-technique → More specific implementation
```

### The 14 ATT&CK Tactics (Enterprise)

| # | Tactic | Description |
|---|--------|-------------|
| 1 | **Reconnaissance** | Gather information before attacking |
| 2 | **Resource Development** | Establish infrastructure (domains, accounts, tools) |
| 3 | **Initial Access** | Get into the target network |
| 4 | **Execution** | Run malicious code |
| 5 | **Persistence** | Maintain foothold |
| 6 | **Privilege Escalation** | Gain higher-level permissions |
| 7 | **Defence Evasion** | Avoid detection |
| 8 | **Credential Access** | Steal credentials |
| 9 | **Discovery** | Explore the environment |
| 10 | **Lateral Movement** | Move through the network |
| 11 | **Collection** | Gather data of interest |
| 12 | **Command & Control (C2)** | Communicate with compromised systems |
| 13 | **Exfiltration** | Steal data |
| 14 | **Impact** | Destroy, encrypt, or disrupt |

### Key Techniques to Know for CySA+

```
T1078 — Valid Accounts (Persistence, Privilege Escalation)
         Using stolen/compromised legitimate credentials

T1059 — Command and Scripting Interpreter (Execution)
         PowerShell, Bash, Python, VBScript

T1055 — Process Injection (Privilege Escalation, Defence Evasion)
         Injecting code into another process's memory

T1003 — OS Credential Dumping (Credential Access)
         Mimikatz, LSASS dumping

T1021 — Remote Services (Lateral Movement)
         RDP, SMB, SSH, WMI

T1053 — Scheduled Task/Job (Persistence)
         Creating scheduled tasks for persistence

T1083 — File and Directory Discovery (Discovery)
         Enumeration of file system

T1041 — Exfiltration Over C2 Channel (Exfiltration)
         Data sent via existing C2 channel

T1486 — Data Encrypted for Impact (Impact)
         Ransomware encryption

T1566 — Phishing (Initial Access)
         Email phishing, spear phishing
```

---

## Threat Hunting (Exam Objective 1.4)

**Threat hunting** is the proactive, human-led search for threats that have evaded automated detection. Unlike alert-driven analysis, hunting starts with a hypothesis.

### Threat Hunting Process

```
1. HYPOTHESIS
   "Attackers using DNS tunnelling to exfiltrate data"
   
2. DATA COLLECTION
   Pull DNS logs, NetFlow, endpoint telemetry

3. HUNT EXECUTION
   Query for long subdomain labels, high NXDomain rates,
   unusual query volumes per host

4. ANALYSIS
   Correlate findings, eliminate false positives

5. CONCLUSION
   Found: suspicious DNS to new domain from finance workstation
   Not found: confirm hypothesis was wrong (valuable too)

6. DOCUMENT & IMPROVE
   Add detection rule to SIEM, update threat model
```

### Hypothesis Sources
- **Intel-driven:** Based on CTI reports ("APT29 is targeting our sector")
- **TTP-driven:** Based on MITRE ATT&CK ("hunt for T1021 lateral movement")
- **Anomaly-driven:** Based on unusual patterns ("host with 10x normal DNS queries")

### Hunting Queries — Examples
```bash
# Hunt for DNS tunnelling (Splunk SPL):
index=dns | stats count by query | where count < 5 AND len(query) > 50

# Hunt for scheduled tasks created by non-admin:
index=winevent EventCode=4698 | where NOT user IN ("Administrator","SYSTEM")

# Hunt for lateral movement via RDP:
index=winevent EventCode=4624 LogonType=10 
| stats count by src_ip, dest_ip 
| where src_ip != dest_ip

# Hunt for PowerShell encoded commands (common obfuscation):
index=winevent EventCode=4688 
| where like(CommandLine, "%-enc%") OR like(CommandLine, "%-EncodedCommand%")
```

---

## Threat Actor Categories

| Type | Motivation | Capability | Examples |
|------|------------|------------|---------|
| **Nation-state APT** | Espionage, sabotage | Very high | Lazarus (NK), APT28 (RU), APT41 (CN) |
| **Criminal/Ransomware** | Financial gain | High | LockBit, REvil, Cl0p |
| **Hacktivist** | Political/ideological | Medium | Anonymous |
| **Insider threat** | Revenge, money, ideology | Varies | Disgruntled employees |
| **Script kiddie** | Fame, curiosity | Low | Using pre-built tools |

### Threat Actor Attributes
- **TTPs:** Tactics, Techniques, Procedures — fingerprint of how an actor operates
- **IOCs:** Indicators specific to that actor
- **Motivation:** Financial, political, ideological, espionage

---

## Practice Questions

1. What is the difference between strategic and tactical threat intelligence?
2. Which framework maps adversary behaviour into tactics and techniques?
3. A threat hunter suspects DNS tunnelling. What data source would they query first?
4. What protocols are used to share threat intelligence in a standardised, automated way?
5. MITRE ATT&CK technique T1003 involves what type of activity?

**Answers:**
1. Strategic = high-level for executives (trends, geopolitics); Tactical = IoC-level for analysts (IPs, hashes, domains)
2. MITRE ATT&CK
3. DNS logs
4. STIX (format) and TAXII (transport protocol)
5. OS Credential Dumping — stealing credentials from LSASS, SAM database (e.g., using Mimikatz)
