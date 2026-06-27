# Day 12 — Digital Forensics & Evidence Handling

## Domain 3.0 — Incident Response and Management (Objective 3.2)

## Digital Forensics Overview

**Digital forensics** is the process of collecting, preserving, analysing, and presenting digital evidence in a manner that is legally admissible and scientifically sound.

### The Forensic Process
```
1. IDENTIFICATION  → Recognise potential evidence sources
2. PRESERVATION    → Secure and protect evidence (chain of custody)
3. COLLECTION      → Acquire evidence without alteration
4. ANALYSIS        → Examine evidence for findings
5. REPORTING       → Document findings in clear, supported language
6. PRESENTATION    → Testify or present findings
```

---

## Order of Volatility

Evidence must be collected from MOST volatile to LEAST volatile:

```
MOST VOLATILE (collect first — lost on power loss/reboot)
  1. CPU registers and cache
  2. RAM (system memory)
  3. Network connections (active sessions, ARP tables)
  4. Running processes
  5. Open files (handles)
  6. Logged-in users

LESS VOLATILE (survives reboot)
  7. Disk contents (files, logs)
  8. Flash drives, USB devices
  9. CD/DVD

LEAST VOLATILE
  10. Remote logs / SIEM (centralised logging)
  11. Paper records
  12. Archival media (tapes, long-term backups)
```

**Exam tip:** If asked "what should the analyst collect FIRST?" — the answer is always the most volatile source (RAM/memory).

---

## Memory Forensics

RAM contains critical evidence: running processes, encryption keys, network connections, malware code (fileless malware only lives in memory).

```bash
# Memory acquisition tools:
# Volatility Framework (free, industry standard)
# WinPmem / DumpIt (Windows memory acquisition)
# LiME (Linux Memory Extractor)
# FTK Imager (Windows)
# Magnet RAM Capture (Windows, free)

# Volatility 3 commands (Linux):
# List running processes
python3 vol.py -f memory.dmp windows.pslist

# Show process tree (parent-child relationships)
python3 vol.py -f memory.dmp windows.pstree

# Network connections at time of capture
python3 vol.py -f memory.dmp windows.netstat

# Detect process injection
python3 vol.py -f memory.dmp windows.malfind

# Dump process memory
python3 vol.py -f memory.dmp windows.dumpfiles --pid 1234

# Find command history
python3 vol.py -f memory.dmp windows.cmdline
```

---

## Disk Forensics

### Write Blockers (CRITICAL concept)
Before imaging a disk: **attach a write blocker** to prevent any modification of the evidence.
- Hardware write blocker: physical device (Tableau, WiebeTech)
- Software write blocker: OS-level protection
- WITHOUT a write blocker: accessing the drive modifies access timestamps → evidence contamination

### Disk Imaging (Bit-for-bit copy)
```bash
# dd — create forensic disk image (Linux)
dd if=/dev/sdb of=/mnt/evidence/disk.img bs=512 conv=noerror,sync

# dcfldd — dd with hashing and verification
dcfldd if=/dev/sdb of=/mnt/evidence/disk.img hash=sha256 hashlog=hash.log

# dd with hash verification
dd if=/dev/sdb of=disk.img
sha256sum /dev/sdb > hash.txt      # Hash BEFORE imaging
sha256sum disk.img >> hash.txt     # Hash AFTER — must match!

# FTK Imager (Windows) — GUI tool for forensic imaging
# Creates .E01 (EnCase) or .001 (raw) images

# Mount an image READ-ONLY for analysis
mount -o ro,loop disk.img /mnt/evidence
```

### File System Forensics
```bash
# Autopsy — GUI forensics platform (industry standard)
# Sleuth Kit — command-line tools

# Key Sleuth Kit commands:
fls disk.img             # List files (including deleted)
icat disk.img 12345      # Extract file by inode number
fsstat disk.img          # File system statistics
mmls disk.img            # Partition table analysis

# File recovery:
# Deleted files remain on disk until overwritten
# "Carving" — recover files by file signatures (magic bytes)
# Tools: Autopsy, Scalpel, PhotoRec, Foremost

# Key forensic artifacts to examine:
# Windows:
#   $MFT (Master File Table) — file metadata
#   $RECYCLE.BIN — deleted files
#   LNK files (link files) — recently opened files
#   Prefetch files — evidence of program execution
#   Windows Registry — configuration, user activity, persistence
#   Event Logs — security events, application events
#   NTFS Alternate Data Streams (ADS) — hidden data

# Linux:
#   /var/log/ — system logs
#   ~/.bash_history — command history
#   /etc/passwd, /etc/shadow — user accounts
#   /tmp — often used by malware
#   Cron jobs — persistence
#   Deleted file recovery via inode analysis
```

---

## Chain of Custody

A documented record proving evidence integrity from collection to presentation.

### Chain of Custody Requirements
```
For each piece of evidence document:
□ Description of the item (make, model, serial number, capacity)
□ Date and time collected
□ Who collected it (name, role, badge number)
□ Where it was collected (physical location)
□ Who it was transferred to and when
□ Storage location
□ Hash values (MD5 and SHA256) at collection and at each transfer
□ Seal integrity (evidence bag serial numbers)
```

### Evidence Handling Best Practices
- **Photograph** the device in place before touching it
- Use **evidence bags** with tamper-evident seals
- Label every item with unique identifier
- Maintain **temperature-controlled storage** for long-term
- Limit access to evidence — need-to-know only
- Document every time evidence is accessed

---

## Network Forensics

```bash
# Capture live traffic:
tcpdump -i eth0 -w evidence.pcap
Wireshark (GUI)
Zeek (structured logs)

# Analyse PCAPs:
Wireshark — full packet analysis
NetworkMiner — extract files from PCAP
Zeek — parse into structured logs
ngrep — grep for strings in network traffic

# NetFlow analysis for historical traffic:
nfdump -R /var/log/netflow -s srcip/bytes   # Top sources by bytes
nfdump -R /var/log/netflow "host 10.0.1.50" # Traffic to/from specific host
```

---

## Log Forensics

```bash
# Windows Event Log analysis:
Get-WinEvent -LogName Security | Where {$_.Id -eq 4624}  # Logons
Get-WinEvent -LogName Security | Where {$_.Id -eq 4688}  # Processes

# PowerShell investigation:
Get-WinEvent "Microsoft-Windows-PowerShell/Operational" | 
  Where {$_.Message -match "EncodedCommand"}

# Linux log forensics:
# Reconstruct timeline of events:
aureport --summary    # auditd summary
ausearch -i -ts today # Today's audit events
last | head -20       # Recent logins

# Log correlation across sources:
# Always build a timeline showing:
# Time → Source → Event → Username → System
```

---

## Practice Questions

1. An analyst needs to acquire evidence from a running server. What should they collect FIRST?
2. Before imaging a suspect hard drive, what MUST be in place to prevent evidence contamination?
3. What is chain of custody and why does it matter?
4. A forensic analyst finds a file that was "deleted" on a Windows NTFS system. Where might they recover it from?
5. What hashing algorithm is preferred for forensic evidence integrity verification?

**Answers:**
1. Memory (RAM) — most volatile, contains running processes, network connections, possible encryption keys
2. A write blocker — prevents any writes to the evidence drive during acquisition
3. Chain of custody documents who handled evidence, when, and how — maintains admissibility in legal proceedings
4. The $RECYCLE.BIN, via inode analysis using Sleuth Kit (deleted files remain until overwritten), or from a shadow copy/VSS snapshot
5. SHA-256 — MD5 is faster but has known collision vulnerabilities; SHA-256 provides stronger integrity assurance
