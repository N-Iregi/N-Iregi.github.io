<!-- 
  === NEVILLE IREGI – CLOUD BREAKER & FULL STACK LEARNING LOGS ===
  ALU Software Engineering | Full-Stack → Cloud Pentester Path
  Log Entry: January 2026
-->

<div align="center">
  <img src="https://img.shields.io/badge/Log%20Status-Active%20Recon-orange?style=for-the-badge&logo=aws&logoColor=white" alt="Log Status" />
  <img src="https://img.shields.io/badge/Mission-Build%20to%20Break-blue?style=for-the-badge" alt="Mission" />
  <br><br>
  <h1>Cloud Breaker Logs ☁️💥</h1>
  <h3>By NEVILLE IREGI – ALU Full-Stack Student & Aspiring Cloud Red Teamer</h3>
</div>

These are my field notes from the front lines of cloud security:  
**deploying apps → hunting misconfigs → chaining exploits → hardening defenses**.  
As an ALU software engineering student, I build full-stack systems to understand how they shatter under pressure. My focus is real-world cloud attacks, with side detours into low-level memory tricks that could bleed into cloud edges.

**Why this portfolio?** To show I'm not just coding — I'm thinking like an attacker who fixes what they find.

### 🕵️‍♂️ Recon: My Current Focus Areas (Q1 2026)

- **IAM Escalation Games**: Turning over-privileged roles into full account dominance.
- **Serverless Shadows**: Lambda injections and runtime quirks that hide backdoors.
- **API Frontier**: SSRF paths that lead to metadata goldmines.
- **Pipeline Perils**: CI/CD leaks in GitHub Actions/Terraform that cascade to compromise.
- **Container Labyrinths**: K8s RBAC slips and pod escapes.
- **Low-Level Echoes**: Buffer overflows/ROP as "what if" bridges to cloud firmware vulns.

### 📓 Field Logs: Key Exploits & Chains

Think of these as ops reports from my lab sessions. Each includes setup code, attack PoC, evasion notes, and fixes.

| Log ID | Entry Title | Target Environment | Attack Narrative | Impact | Link / Artifacts |
|--------|-------------|--------------------|------------------|--------|------------------|
| LOG-001 | "JWT Shadow Play" | React/Node + Cognito | Forged 'none' alg token → IDOR slip → admin dashboard hijack → S3 data dump. | High: Full user impersonation + PII exfil. | [📄 Ops Report + PoC Code](log-001-jwt-shadow) – GIF demo included. |
| LOG-002 | "SSRF Whisper Network" | API Gateway + Lambda | Image upload endpoint → silent SSRF to metadata → IAM temp keys harvest. | Critical: Cross-account privesc. | [📄 Full Log](log-002-ssrf-whisper) – Video walkthrough (3 min). |
| LOG-003 | "Lambda Phantom Role" | Serverless + IAM | Over-permissive execution → sts:AssumeRole chain → resource deletion spree. | High: Persistence via backdoored functions. | [In Progress](log-003-lambda-phantom) – Python script PoC. |
| LOG-004 | "Pipeline Ghost in the Machine" | GitHub Actions + Terraform | Workflow secret leak → AWS credential reuse → infra drift exploitation. | Medium: Supply chain compromise. | Planned – Q1 2026. |
| LOG-005 | "K8s Labyrinth Escape" | Docker/K8s Pod | Misconfigured ServiceAccount → token theft → cluster admin. | Critical: Full environment pivot. | Planned – Q1 2026. |
| SIDE-001 | "Memory Echo in the Cloud" | C Binary + Hypothetical Cloud Edge | Buffer overflow → ROP gadget chain → simulated metadata leak. | Low (learning): Bridge to IoT/cloud firmware. | [📄 Side Note](side-001-memory-echo) – OverTheWire-inspired. |

**Log Format Notes**: Each entry has vulnerable code (e.g., Terraform for infra), exploit scripts (Python/Bash), CloudTrail evasion tips, and remediation PRs. I prioritize chains over isolated bugs — because real attacks aren't single steps.

### ⚙️ Gear & Arsenal

<div align="center">
  <img src="https://img.shields.io/badge/AWS-FF9900?style=flat&logo=amazon-aws&logoColor=white" />
  <img
  src="https://img.shields.io/badge/Microsoft%20Azure-0089D6?style=flat&logo=microsoft-azure&logoColor=white"
  />
  <img src="https://img.shields.io/badge/Wireshark-1679A7?style=flat&logo=wireshark&logoColor=white" />
  <img src="https://img.shields.io/badge/Bash-4EAA25?style=flat&logo=gnubash&logoColor=white" />
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/Node.js-43853D?style=flat&logo=node.js&logoColor=white" />
  <img src="https://img.shields.io/badge/React-61DAFB?style=flat&logo=react&logoColor=black" />
  <img src="https://img.shields.io/badge/Terraform-7B42BC?style=flat&logo=terraform&logoColor=white" />
  <img src="https://img.shields.io/badge/Pacu-FF6F61?style=flat" />
  <img src="https://img.shields.io/badge/CloudGoat-F4A261?style=flat" />
  <img src="https://img.shields.io/badge/BurpSuite-FF0000?style=flat&logo=burp-suite&logoColor=white" />
  <img src="https://img.shields.io/badge/LowLevel-OverTheWire-9cf?style=flat" />
</div>

### 🗺️ Mission Log: My Path Forward

**Entry 0: Origin**  
Started with ALU Full-Stack (React, Node, Python, DevOps) — learned to build scalable apps. Then my Cloud & Network Security course opened the door to breaking them.

**Current Coordinates**  
Honing exploits in AWS free tier labs; side-dipping into low-level pwn for that extra edge.

**Next Milestones**  
- Q1: AWS Security Specialty cert  
- Q2: 2–3 bug bounty triages (AWS focus)  
- Q3: Entry-level cloud pentest internship/role  

**Philosophy**: Security is storytelling — what went wrong, how it chains, and how to rewrite the ending.

### 📡 Signal Check: Let's Connect

If my logs resonate, or if you're scouting juniors who can deploy, exploit, and defend:  
- Share a misconfig war story  
- Offer feedback on a chain  
- Discuss entry opportunities in cloud pentest / AppSec  

Open to remote roles at startups pushing cloud security (e.g., Wiz, Snyk) or African tech scaling defenses.

💬 LinkedIn DM: linkedin.com/in/neville-iregi  
📧 n.iregi@alustudent.com  
🌐 [your-twitter-or-x-handle] (if active)

<div align="center">
  <i>"In the cloud, every misconfig is a story waiting to unfold — I find them before they become headlines."</i>
</div>
