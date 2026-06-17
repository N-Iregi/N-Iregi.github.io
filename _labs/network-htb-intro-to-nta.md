---
title: "HTB Academy: Introduction to Network Traffic Analysis"
layout: single
author_profile: true
toc: true
toc_sticky: true
---

# Overview

Network Traffic Analysis (NTA) is a core skill for both offensive and defensive security practitioners. This module covered the principles of NTA and provided hands-on practice with tcpdump and Wireshark across multiple lab scenarios — from basic traffic capture to live incident analysis and RDP decryption.

Offensive uses: hunting for credentials, hidden services, reachable network segments, and cleartext data. Defensive uses: baselining the network, detecting anomalies, and reconstructing attack timelines from PCAP evidence.

# Tools & Environment

- tcpdump (CLI-based, Linux/UNIX)
- Wireshark (GUI-based)
- TShark / Termshark (terminal Wireshark variants)
- HTB Academy lab environment (XfreeRDP access)
- Packet capture files: `.pcap` / `.pcapng`

# Part 1: tcpdump Fundamentals

## Key Switches

| Switch | Result |
|--------|--------|
| `-D` | List available capture interfaces |
| `-i` | Select interface (e.g. `-i eth0`) |
| `-n` / `-nn` | Disable hostname / port resolution (best practice — faster, no DNS delay) |
| `-v / -vv / -vvv` | Increase output verbosity |
| `-X` | Show packet contents in Hex and ASCII |
| `-XX` | Same as `-X` plus Ethernet headers |
| `-c` | Capture a specific number of packets then stop |
| `-w` | Write capture to `.pcap` file |
| `-r` | Read from `.pcap` file |
| `-l` | Line-buffered stdout — enables piping to `grep` |
| `-S` | Show absolute sequence numbers |
| `-A` | Show ASCII only (no hex) |

**Best practice capture command:**
```bash
sudo tcpdump -i eth0 -nvXc 100 -w /path/to/capture.pcap
```

**Read and analyze a saved capture:**
```bash
sudo tcpdump -nnSXr /tmp/capture.pcap
```

**Pipe capture to grep (e.g. scrape emails):**
```bash
sudo tcpdump -Ar http.cap -l | grep 'mailto:*'
```

## Packet Filters

| Filter | Effect |
|--------|--------|
| `host` | Bi-directional filter on a specific host |
| `src` / `dst` | Filter by source or destination |
| `net` | Filter by network (CIDR notation) |
| `port` | Bi-directional port filter |
| `portrange` | Filter a port range (e.g. `0-1024`) |
| `proto` | Filter by protocol (tcp, udp, icmp, ether) |
| `not` | Exclude matching traffic (e.g. `not icmp`) |
| `and` / `or` | Combine filters |

**Pre-capture filters** drop non-matching traffic permanently — best for targeted troubleshooting. **Post-capture filters** applied during file reading leave the original capture intact while narrowing terminal output.

## Looking for TCP Flags

```bash
# Show only SYN packets
sudo tcpdump -i eth0 'tcp[13] &2 != 0'
```

`tcp[13]` references byte offset 13 of the TCP header (flags field). `&2` applies a bitwise AND against the SYN bit. Non-zero result means the flag is set.

# Part 2: tcpdump Labs

## Lab 1 — Fundamentals

**Scenario:** Capture local broadcast domain traffic as a baseline for the corporate network.

Key tasks and commands:
```bash
which tcpdump                              # verify installation
sudo tcpdump -D                            # list interfaces
sudo tcpdump -i eth0 -vX                   # verbose + hex/ASCII
sudo tcpdump -i eth0 -nn                   # disable name resolution
sudo tcpdump -i eth0 -nvw capture.pcap     # write to file
sudo tcpdump -nnSXr capture.pcap           # read back with absolute seq numbers
```

## Lab 2 — Interrogating Traffic with Capture and Display Filters

**Scenario:** Analyze a corporate PCAP to identify DNS and HTTP/S servers.

Key findings from `TCPDump-lab-2.pcap`:

- Protocols present: DNS (UDP/53), HTTP (TCP/80), HTTPS (TCP/443), UDP/1337
- First TCP three-way handshake: client port 43806 → server port 80
- DNS server: `172.16.146.1`
- Apache.org resolved to: `95.216.26.30` and `207.244.88.140`
- First established connection timestamp: `18:34:01.401270`
- Most common HTTP method: POST
- Most common HTTP response: 200 OK
- First conversation protocol: HTTP/80
- DNS record types observed: A (IPv4), AAAA (IPv6), CNAME

Server identification rule: servers answer on well-known ports (80, 443, 53). Clients use random high ports.

# Part 3: Wireshark

## GUI vs. Terminal Options

- **Wireshark** — full GUI, deep packet inspection, plugin ecosystem, decryption capabilities (IPsec, TLS, WPA2, Kerberos, SNMPv3)
- **TShark** — terminal-based Wireshark, shares BPF filter syntax with tcpdump, ideal for headless systems or piping output
- **Termshark** — TUI (terminal UI) that mirrors the Wireshark interface in the terminal

## Lab 3 — Wireshark Familiarity

**Scenario:** A user's laptop is making unusual connections and experiencing slow
load times. Capture and analyze traffic to determine if something is wrong.

Key findings from live capture (pepsi.com + apache.org):

- Multiple TCP sessions established to the same server IP using different source ports — browsers open parallel sessions for resource fetching
- Protocols visible: DNS, HTTP, HTTPS/TLS
- HTTP traffic (apache.org, port 80): plaintext — full URL, method, Host header, User-Agent, cookies, response body visible
- HTTPS traffic: encrypted application data, but SNI (Server Name Indication) in the TLS Client Hello leaks the hostname (`tls.handshake.extensions_server_name`)

## Lab 4 — Packet Dissection and File Extraction

**Scenario:** A user is suspected of smuggling data out of the network via images
embedded in HTTP traffic. Extract the files as evidence.

Workflow:
1. Open `Wireshark-lab-2.pcap`
2. Apply display filter: `http`
3. Locate packets with `200 OK` responses
4. Right-click → Follow → TCP Stream to confirm file transfer
5. Filter for JPEG files: `http && image-jfif`
6. Export: File → Export Objects → HTTP → save `.JPEG`

**Key concept:** JFIF (JPEG File Interchange Format) is the marker to look for
when hunting for image files in HTTP traffic.

## Lab 5 — Live Capture and FTP/HTTP Analysis

**Scenario:** Investigate host `172.16.10.2` for suspicious activity.

Key findings:

- FTP server: `172.16.10.20` — anonymous authentication, file transfer detected
- HTTP server: `172.16.10.20` — Apache 2.4.41 on Ubuntu
- Most common HTTP methods: GET and POST
- Image files embedded in HTTP traffic extracted as evidence

FTP file extraction workflow:
1. Filter: `ftp` — examine command controls
2. Filter: `ftp-data` — locate file transfer packets
3. Follow TCP stream → change "Show and save data as" to **Raw** → save with
   original filename

# Part 4: Guided Incident Analysis

**Scenario:** Suspicious traffic from Bob's host (`172.16.10.90`). Investigate
using `guided-analysis.pcap`.

## Analysis Workflow

1. **Define the issue** — suspicious outbound traffic from an internal host
2. **Scope** — host `10.129.43.4`, connections to port 4444, last 48 hours
3. **Filter noise** — isolate UDP first, then TCP

## UDP Traffic

9 packets total: 4 ARP, 4 NAT-PMP, 1 SSDP — all normal network traffic, nothing
suspicious.

## TCP Traffic

Filter: `!udp && !arp`

All remaining TCP traffic is a single conversation between `10.129.43.4` and
`10.129.43.29` over **port 4444** (non-standard). Three-way handshake visible at
packet 3. No TCP teardown or RST — session likely still active at capture time.

**Following the TCP stream revealed:**
`whoami`

`ipconfig`

`dir`

`net user hacker /add`

`net localgroup administrators hacker /add`

A malicious actor was using a **Netcat reverse shell** on port 4444, performing
reconnaissance and creating a local administrator account named `hacker`.

**Summary:** Host `10.129.43.29` compromised. Actor used Bob's host to execute
net commands and establish an RDP session to a second Windows host
(`nta-rdp-srv01\mrb3n`) to establish further foothold. Incident Response
initiated.

# Part 5: Decrypting RDP Traffic

**Scenario:** IR team captured RDP traffic from Bob's host. An RSA private key
was recovered from the host. Use it to decrypt the capture in Wireshark.

## Steps

1. Open `rdp.pcapng` in Wireshark
2. Filter: `tcp.port == 3389` — confirm TCP session established
3. Navigate to: Edit → Preferences → Protocols → TLS
4. RSA Keys List → Add entry:
   - IP: `10.129.43.29`
   - Port: `3389`
   - Protocol: `tpkt`
   - Key file: `server.key`
5. Save and reload PCAP
6. Filter: `rdp` — decrypted RDP packets now visible

**Findings:**
- RDP session initiated by: `10.129.43.27`
- User account used: `bucky` (visible in "Ignored Unknown Record" ASCII output)

**Key concept:** Wireshark can decrypt TLS-wrapped protocols if the RSA private
key used for the session is available. The same approach applies to any
TLS-encrypted protocol given the right key material.

# Key Takeaways

- tcpdump and Wireshark are complementary — tcpdump for headless/remote capture, Wireshark for deep interactive analysis
- `-nn` should be default practice in tcpdump to avoid DNS delays
- Pre-capture filters reduce data volume; post-capture filters preserve evidence while narrowing analysis scope
- HTTP traffic is fully readable in plaintext — credentials, cookies, file transfers all visible
- HTTPS hides content but SNI leaks hostnames in the TLS handshake
- FTP transmits credentials and file contents in cleartext
- Netcat shells on non-standard ports (e.g. 4444) are a strong indicator of compromise when seen in TCP streams
- Wireshark's Export Objects feature enables evidence extraction directly from PCAP files

# Module Completion

🏆 [HTB Academy Certificate — Introduction to Network Traffic Analysis](https://academy.hackthebox.com/achievement/2158671/81)

# Full Technical Report

📄 **Detailed Lab Report**
- [⬇️ Download PDF](/assets/reports/Network-HTB-Academy-Introduction-to-Network-Traffic-Analysis.pdf)
- [👁 View in new tab](/assets/reports/Network-HTB-Academy-Introduction-to-Network-Traffic-Analysis.pdf){:target="_blank"}