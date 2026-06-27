# Day 3 — SIEM, Log Analysis & Threat Detection

## Domain 1.0 — Security Operations

## What Is a SIEM?

A **Security Information and Event Management (SIEM)** system is the central platform in a SOC. It:

1. **Collects** logs from all sources (firewalls, endpoints, servers, apps)
2. **Normalises** them into a common format
3. **Correlates** events across sources to detect patterns
4. **Alerts** analysts when rules are triggered
5. **Stores** logs for investigation and compliance

**Common SIEMs you need to know:**
- **Splunk** — industry leading, search-based (SPL query language)
- **Microsoft Sentinel** — cloud-native, Azure-integrated (KQL query language)
- **IBM QRadar** — enterprise, AQL query language
- **Elastic SIEM / ELK Stack** — open-source, JSON logs, KQL
- **ArcSight** — HP/Micro Focus, legacy enterprise

---

## SIEM Core Concepts

### Log Aggregation vs Correlation
- **Aggregation:** Collecting logs in one place (raw data)
- **Correlation:** Connecting related events across sources to detect attacks
  - *Example:* Failed login (auth log) + new process created (EDR) + outbound connection (firewall) → correlates to credential stuffing → lateral movement → C2

### SIEM Rules / Use Cases
Rules define what patterns trigger an alert. Examples:
```
Rule: "Brute Force Attempt"
Condition: ≥10 failed logins from same source IP in 5 minutes
Action: Create alert, notify Tier 1 analyst

Rule: "Admin Login Outside Business Hours"
Condition: Privileged account login between 22:00 and 06:00
Action: Create medium-severity alert

Rule: "Impossible Travel"
Condition: Same user authenticates from two geographically distant
           locations within 2 hours (impossible travel time)
Action: Create high-severity alert, auto-block
```

### Baseline and Anomalies
- A **baseline** is normal behaviour for a user, system, or network
- An **anomaly** is a deviation from the baseline
- SIEMs using **UEBA (User and Entity Behaviour Analytics)** automatically detect anomalies using ML

---

## Reading and Analysing Logs (Exam Objective 1.2, 1.3)

### Windows Event Log — Key Event IDs

| Event ID | Description | Security Significance |
|----------|-------------|----------------------|
| 4624 | Successful logon | Baseline normal activity |
| 4625 | Failed logon | Brute force if repeated |
| 4648 | Logon using explicit credentials | Pass-the-hash, lateral movement |
| 4672 | Special privileges assigned | Admin/privileged escalation |
| 4688 | New process created | Malware execution, C2 tools |
| 4698 | Scheduled task created | Persistence mechanism |
| 4719 | Audit policy changed | Attacker covering tracks |
| 4720 | User account created | Backdoor account |
| 4776 | NTLM authentication | Lateral movement indicator |
| 7045 | New service installed | Malware persistence |

### Linux / Syslog Key Files
```bash
/var/log/auth.log      # Authentication events (SSH, sudo)
/var/log/syslog        # General system events
/var/log/messages      # System messages (RHEL/CentOS)
/var/log/secure        # Security events (RHEL/CentOS)
/var/log/apache2/      # Web server access/error logs
/var/log/audit/audit.log # SELinux & kernel audit events

# Key patterns to look for:
grep "Failed password" /var/log/auth.log        # SSH brute force
grep "Invalid user" /var/log/auth.log           # Unknown account targeting
grep "sudo" /var/log/auth.log                   # Privilege escalation
grep "COMMAND" /var/log/auth.log | grep root    # Root commands via sudo
```

### Firewall Log Analysis
```
# Example firewall log entry:
2024-06-15 14:23:45 DENY TCP 203.0.113.42:54321 → 10.0.1.50:3389 [Port scan]

Fields: Timestamp | Action | Protocol | Source:Port → Dest:Port | [Reason]

# Red flags in firewall logs:
- Many DENY entries from same source → port scan or brute force
- Internal host connecting to known malicious IP → C2 communication
- Outbound traffic on unusual port (e.g., DNS to non-DNS server) → tunnelling
- Large data transfer to external IP at unusual time → exfiltration
```

### DNS Log Analysis — C2 Detection
```
# Normal DNS:
client → DNS query → google.com → IP returned

# Suspicious DNS patterns:
- High volume of NX (Non-Existent) domain queries → DGA (Domain Generation Algorithm)
- Long subdomain queries → DNS tunnelling (data encoded in subdomain)
  e.g. aGVsbG8gd29ybGQ.malicious.com
- Queries to newly registered domains → malware C2
- Unusually high TTL changes → DNS hijacking
```

---

## Threat Detection Indicators (Exam Objective 1.2)

### Network-Based Indicators
| Indicator | Potential Meaning |
|-----------|------------------|
| Unusual outbound port | C2, tunnelling, exfiltration |
| Large upload to external IP | Data exfiltration |
| Beacon traffic (regular intervals) | C2 heartbeat |
| Traffic to TOR exit nodes | Anonymised C2 |
| Encrypted traffic to unknown host | Malware C2 over TLS |
| Lateral movement (internal scanning) | Post-compromise spread |

### Host-Based Indicators
| Indicator | Potential Meaning |
|-----------|------------------|
| New scheduled task or service | Persistence |
| Process spawning unusual children | Malware execution |
| cmd.exe / powershell.exe from unexpected parent | Living off the land |
| Registry modifications at Run key | Persistence |
| Large number of files accessed/encrypted | Ransomware |
| Outbound connection from word/excel | Macro malware |

### Living Off the Land (LotL) / LOLBins
Attackers use legitimate Windows tools to avoid detection:
- **PowerShell** — script execution, download cradles
- **WMI (wmic.exe)** — remote execution
- **certutil.exe** — download files, decode base64
- **regsvr32.exe** — bypass AppLocker, execute code
- **mshta.exe** — execute HTML applications
- **rundll32.exe** — execute DLLs
- **bitsadmin** — download files
- **msiexec** — install MSI packages silently

---

## SIEM Tuning

Too many false positives? Tune the rules:
- Whitelist known good IPs, users, processes
- Adjust thresholds (e.g., 3 failed logins → 10)
- Add time-based context (business hours only)
- Require multiple conditions (AND logic)

---

## Practice Questions

1. Event ID 4688 appears in logs showing `powershell.exe` spawned by `winword.exe`. What does this likely indicate?
2. DNS queries show `aGVsbG8gd29ybGQ.evil-domain.com` repeated every 60 seconds. What attack technique is this?
3. Your SIEM generates 2000 alerts per day but analysts can only investigate 100. What should you do first?
4. A user's account shows logins from New York at 8am and London at 9am the same day. What SIEM rule detects this?
5. What is the difference between SIEM aggregation and SIEM correlation?

**Answers:**
1. Macro malware — Word document executed PowerShell (LotL)
2. DNS tunnelling — data encoded in subdomain labels
3. Tune SIEM rules to reduce false positives — prioritise high-fidelity alerts
4. Impossible Travel / Geolocation anomaly rule
5. Aggregation = collecting logs; Correlation = connecting events across sources to detect patterns
