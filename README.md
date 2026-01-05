<!-- 
  === NEVILLE IREGI – SOFTWARE ENGINEERING & CYBERSECURITY PORTFOLIO ===
  ALU Software Engineering | Full-Stack Track | Cloud & Network Security
  Last updated: January 2026
-->

<div align="center">
  <img src="https://img.shields.io/badge/Log%20Status-Active%20Recon-orange?style=for-the-badge&logo=aws&logoColor=white" alt="Log Status" />
  <img src="https://img.shields.io/badge/Mission-Build%20to%20Break-blue?style=for-the-badge" alt="Mission" />
  <br><br>
  <h1> SOFTWARE ENGINEERING & CYBERSECURITY PORTFOLIO </h1>
  <h3>By NEVILLE IREGI – ALU Full-Stack Student & Emerging Cloud Red Teamer</h3>
</div>

These are my field notes from building, breaking, and hardening software, cloud, & network systems.  
Started with foundational networking and secure coding → now chaining real cloud exploits.

**Current focus**: AWS IAM escalation, serverless misconfigs, SSRF chains, Azure security posture tools.

## Skills and Toolkit

<div align="center">
  <img src="https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazon-aws&logoColor=white" />
  <img src="https://img.shields.io/badge/Azure-0078D4?style=flat&logo=microsoftazure&logoColor=white" />
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/Node.js-339933?style=flat&logo=nodedotjs&logoColor=white" />
  <img src="https://img.shields.io/badge/React-20232A?style=flat&logo=react&logoColor=61DAFB" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/Cisco%20Packet%20Tracer-007ACC?style=flat&logo=cisco&logoColor=white" />
  <img src="https://img.shields.io/badge/Bash-4EAA25?style=flat&logo=gnubash&logoColor=white" />
  <img src="https://img.shields.io/badge/Wireshark-1679A7?style=flat&logo=wireshark&logoColor=white" />
</div>

## 🕵️‍♂️ Active Recon Surfaces (Q1 2026)

- IAM privilege escalation & rollback attacks
- Serverless injection & Lambda role abuse
- Azure observability & proactive defense (Monitor, Defender, Sentinel, JIT)
- API abuse patterns & metadata leaks
- Low-level memory fundamentals (side exploration)

## Key Exploits, Labs & Projects

| Log ID | Entry Title | Environment / Tools | Attack / Achievement Narrative | Skills Demonstrated | Link / Status |
|--------|-------------|---------------------|--------------------------------|---------------------|---------------|
| LOG-009 | Azure Security & Observability Stack | Azure | Implemented Azure Monitor (logs/metrics), Defender for Cloud (posture), JIT VM access (attack surface reduction), Sentinel (threat detection & investigation). Hands-on cloud ops & proactive mitigation. | Azure Monitor · Microsoft Defender for Cloud · Microsoft Sentinel · Just-In-Time Access | [PDF Report](Azure-Monitor-Defender-JIT-Sentinel.pdf) – Dec 2025 |
| LOG-008 | CloudGoat Vulnerable Lambda | AWS Lambda + IAM | Enumerated IAM → assumed privileged roles (STS) → analyzed vulnerable Lambda code → exploited SQL injection → privilege escalation + secret retrieval. Real serverless attack simulation. | AWS Lambda · AWS IAM · SQL Injection · AWS Security · STS | [PDF Report](CloudGoat-Vulnerable-Lambda.pdf) – Dec 2025 |
| LOG-007 | Azure Key Vault + Always Encrypted | Azure SQL + Entra ID | Built PoC app using Key Vault for key retrieval via Entra ID → protected sensitive columns with Always Encrypted → end-to-end encryption demo. Strong key management & secure app design. | Azure Key Vault · Encryption · Azure SQL Database · Microsoft Entra ID · Always Encrypted | [PDF Report](Key-Vault-And-Encryption.pdf) – Dec 2025 |
| LOG-006 | CloudGoat IAM Privilege Escalation by Rollback | AWS IAM | Simulated rollback exploit → exploited misconfigured policies → escalated privileges. Deepened understanding of least-privilege failures in AWS. | AWS IAM · Privilege Escalation · AWS Security | [PDF Report](CloudGoat-IAM-Rollback.pdf) – Nov 2025 |
| LOG-005 | Quick-Open Vote System | Node.js + Express + SQLite + React-style frontend | Team-built full-stack voting app with real-time results, secure modes (email/official), duplicate prevention, session control, admin dashboard. Production-ready transparency & reliability. | Node.js · RESTful API · SQL & Database · React.js · Docker | [GitHub Repo](https://github.com/N-Iregi/Quick-Open-Vote-system) – Oct–Nov 2025 |
| LOG-004 | Site-to-Site IPsec VPN Configuration | Cisco Packet Tracer | Configured IPsec tunnels, encryption/hashing, IKE/ISAKMP, Diffie-Hellman → secure remote network communication over Internet. | VPN · Cisco Routers · ISAKMP | [PDF Report](Configuring-Site-to-Site-VPNs.pdf) – Oct 2025 |
| LOG-003 | VLANs & Secure Switch Configuration | Cisco Packet Tracer | Designed VLANs for segmentation → inter-VLAN routing → port security, pruning, ACLs → protected against unauthorized access. | Network Switches · VLAN · Port Security | [PDF Report](VLANs-and-Secure-Switch-Configuration.pdf) – Oct 2025 |
| LOG-002 | Switch & Router Enterprise Network | Cisco Packet Tracer | Built multi-switch/router topology → LAN/WAN, IP addressing, RIP/OSPF routing, VLANs → tested connectivity & troubleshooting. | Network Switches · LAN · Router Configuration · Packet Tracer · Cisco Routers · IP Networking | [PDF Report](Build-a-Switch-and-Router-Network.pdf) – Sep 2025 |
| LOG-001 | WLAN Configuration (Home & Enterprise) | Cisco Packet Tracer | Configured SSID, WPA2/WPA3, DHCP, WLC → secure wireless connectivity & troubleshooting across environments. | WLAN · DHCP · WPA · WLC · IP Networking | [PDF Report](Packet-Tracer-WLAN-configuration.pdf) – Sep 2025 |
| SIDE-001 | Threat Detector CLI API | Python + Docker + HAProxy | Built ThreatFox-integrated CLI API → query/submit IOCs → regex validation → load-balanced deployment (Web01/02 + Lb01). Scalable threat intel tool. | Python · API Integration · Docker · HAProxy · DevOps · Regex | [GitHub Repo](https://github.com/N-Iregi/threat_detector) – Jun–Aug 2025 |
| SIDE-002 | Regex Data Extraction Tool | Python | Automated extraction of emails, URLs, phones, credit cards, timestamps from unstructured text → reusable module with test cases. | Python · Regular Expressions · Problem Solving | [GitHub Repo](https://github.com/N-Iregi/alu_regex-data-extraction-N-Iregi) – May–Jun 2025 |
| ORIGIN | Low-Level Programming & Reverse Engineering | C + Assembly + GDB | Memory management, pointers, strings → custom keygen → gdb debugging → binary RE → foundation for exploit dev. | C · x86 Assembly · Memory Management · GDB · Reverse Engineering | [GitHub Repo](https://github.com/N-Iregi/alx-low_level_programming) – Jan 2023–Jan 2024 |


## 🗺️ Mission Trajectory

**Origin → Foundations** (2023–2025)  
Low-level C/assembly → networking (VLANs, VPNs, WLAN) → secure coding & threat intel tools.

**Current Position** (2025–2026)  
Full-Stack apps → AWS/Azure cloud exploits → IAM/serverless chains → proactive defense (Defender, Sentinel, JIT).

**Next Checkpoints**  
- AWS Security Specialty  
- 2–3 bug bounty submissions  
- Entry-level cloud pentest / AppSec role

**Philosophy**: Every misconfiguration is a lesson. Every exploit is a story. Every fix makes the system stronger.

## Let's Connect

Open to:  
• Feedback on labs/write-ups  
• Entry-level cloud security / pentest / AppSec roles  
• Discussions on cloud misconfigs or attack chains

💬 LinkedIn: linkedin.com/in/neville-iregi  
📧 n.iregi@alustudent.com  
🌍 Remote-friendly | Based in Nairobi

<div align="center">
  <i>“I break clouds in labs so they don’t break in production.”</i>
</div>
