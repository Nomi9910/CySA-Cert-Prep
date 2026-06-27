# Day 4 — Network Traffic Analysis & Packet Inspection

## Domain 1.0 — Security Operations

## Why Network Traffic Analysis?

Network traffic analysis (NTA) lets you see what's actually happening on the wire — catching attacks that bypass endpoint detection, identifying C2 communication, and detecting data exfiltration. CySA+ exam expects you to interpret network data and identify malicious patterns.

---

## Key Tools

### Wireshark — Packet-Level Analysis
The standard tool for capturing and analysing network packets.

```
# Key Wireshark display filters for security analysis:

# Show only HTTP traffic
http

# Show DNS queries
dns

# Show all traffic to/from suspicious IP
ip.addr == 203.0.113.42

# Show TCP SYN packets only (port scans)
tcp.flags.syn == 1 and tcp.flags.ack == 0

# Show large payloads (possible exfiltration)
frame.len > 10000

# Show non-standard HTTP ports
http and tcp.port != 80 and tcp.port != 443

# Show failed TCP connections (RST flag)
tcp.flags.reset == 1

# Show POST requests (data submission)
http.request.method == "POST"

# Detect cleartext credentials
http contains "password"
```

### tcpdump — Command-Line Packet Capture
```bash
# Capture all traffic on interface eth0
tcpdump -i eth0

# Capture to file for Wireshark analysis
tcpdump -i eth0 -w capture.pcap

# Filter by host
tcpdump -i eth0 host 203.0.113.42

# Filter by port
tcpdump -i eth0 port 443

# Capture DNS traffic
tcpdump -i eth0 port 53

# Capture and display ASCII/hex content
tcpdump -i eth0 -A host 203.0.113.42

# Capture specific subnet
tcpdump -i eth0 net 192.168.1.0/24

# Capture traffic between two hosts
tcpdump -i eth0 src 192.168.1.10 and dst 8.8.8.8
```

### Zeek (formerly Bro) — Network Log Analysis
Zeek parses network traffic and generates structured logs:
```
# Key Zeek log files:
conn.log      — all connection records (src, dst, port, bytes, duration)
dns.log       — DNS queries and responses
http.log      — HTTP requests and responses
ssl.log       — TLS/SSL certificate information
files.log     — file transfers detected
notice.log    — Zeek detections and alerts
weird.log     — protocol anomalies
```

### NetFlow / IPFIX / sFlow
```
# NetFlow provides flow-level data (not full packet content):
Source IP | Dest IP | Source Port | Dest Port | Protocol | Bytes | Packets | Duration

# Tools: nfdump, ntopng, SolarWinds, ManageEngine

# Detecting anomalies with NetFlow:
- Unusually long sessions → possible C2 or exfiltration
- Large data transfer to external IP → data theft
- Internal host scanning many ports → reconnaissance
- Traffic between unexpected internal segments → lateral movement
```

---

## Protocol-Level Knowledge for the Exam

### TCP Flags
| Flag | Meaning | Attack Context |
|------|---------|----------------|
| SYN | Synchronise (connection initiation) | SYN flood, port scan |
| ACK | Acknowledge | Normal response |
| RST | Reset (abort connection) | Port closed, IDS blocking |
| FIN | Finish (graceful close) | Normal teardown |
| PSH | Push data immediately | Data transfer |
| URG | Urgent data | Rarely legitimate |

### Port Scan Types
| Scan Type | Description | How to Detect |
|-----------|-------------|---------------|
| TCP Connect | Full 3-way handshake | Many complete connections, different ports |
| SYN Scan | Half-open scan | Many SYN with no follow-up |
| UDP Scan | UDP packets to many ports | Many ICMP port-unreachable responses |
| Xmas Scan | FIN+PSH+URG flags set | Unusual flag combination in logs |
| NULL Scan | No flags set | Unusual in firewall logs |
| Idle/Zombie | Spoofed source IP | Difficult to detect directly |

### Suspicious Network Patterns

#### Beaconing (C2 Communication)
```
# Characteristics:
- Regular intervals (e.g., every 60 seconds)
- Small, consistent packet sizes
- Often over HTTP/HTTPS to blend in
- May use domain fronting or CDN IPs

# Detection:
- Plot connection frequency over time → regular spikes = beacon
- Look for connections to newly registered domains
- JA3/JA3S TLS fingerprinting to identify malicious clients
```

#### Data Exfiltration Patterns
```
# HTTP exfiltration:
- Large POST requests to unusual destinations
- HTTP GET with large URL parameters (data in URL)
- High upload:download ratio

# DNS exfiltration:
- Queries with very long subdomain labels
- High frequency of unique domain queries
- NX responses (domain doesn't exist)
- Large TXT record responses

# ICMP tunnelling:
- ICMP packets larger than 64 bytes
- Unusual ICMP type/code combinations
- High volume of ICMP traffic

# Common exfiltration ports:
443 (HTTPS), 80 (HTTP), 53 (DNS), 123 (NTP), 25 (SMTP)
```

---

## Analysing a PCAP — Exam Scenario Approach

When the exam shows you packet data, look for:

1. **Source and destination IPs** — internal? external? known bad?
2. **Ports** — expected for the protocol? unusual outbound?
3. **Volume** — abnormally large? beaconing pattern?
4. **Protocol** — does traffic on port X actually look like protocol X?
5. **Timing** — regular intervals? after-hours?
6. **Content** (if available) — credentials in cleartext? encoded data?

---

## Key Protocols and Attack Vectors

| Protocol | Port | Common Attack |
|----------|------|---------------|
| HTTP | 80 | SQLi, XSS, web shells |
| HTTPS | 443 | C2 over TLS, MITM |
| DNS | 53 | Tunnelling, cache poisoning, DGA |
| SMB | 445 | EternalBlue, Pass-the-Hash, lateral movement |
| RDP | 3389 | Brute force, BlueKeep |
| SSH | 22 | Brute force, key theft |
| SMTP | 25 | Phishing, open relay, exfiltration |
| LDAP | 389/636 | LDAP injection, enumeration |
| Kerberos | 88 | Kerberoasting, Golden/Silver ticket |
| WinRM | 5985/5986 | Lateral movement (PowerShell remoting) |

---

## Practice Questions

1. A Wireshark capture shows repeated SYN packets to ports 21, 22, 23, 25, 80, 443 from the same source. What is happening?
2. NetFlow data shows 2GB transferred outbound to an external IP on port 443 at 3am. What should you investigate first?
3. DNS logs show queries for `dGVzdA.c2server.xyz` every 30 seconds. What technique is the attacker using?
4. TCP RST packets flood from a single destination IP. What might this indicate?
5. What Wireshark filter would show all DNS traffic?

**Answers:**
1. Port scan (sequential SYN to multiple ports)
2. Possible data exfiltration — investigate the destination IP, data content, and who initiated the connection
3. DNS tunnelling — data encoded in subdomain labels
4. The destination is actively refusing connections (port closed), or an IDS/IPS is sending RSTs to block traffic
5. `dns`
