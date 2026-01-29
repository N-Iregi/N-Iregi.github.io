---
title: Lab Challenges
permalink: /labs/
---

## 🔐 Lab Challenges & Security Write-Ups

---

### 🧪 CloudGoat – Vulnerable Lambda (Serverless Privilege Escalation)
**Category:** Cloud Security · AWS · Serverless  

**Problem Statement**
Assess the security of an AWS serverless environment where a low-privilege IAM user has permission to assume a Lambda-invoking role. The objective was to identify IAM and Lambda misconfigurations, escalate privileges, and retrieve a protected secret from AWS Secrets Manager.

**Attack Path**
- Enumerated IAM permissions of the initial user (`bilbo`)
- Identified an assumable IAM role with wildcard permissions
- Assumed the role using AWS STS temporary credentials
- Enumerated Lambda functions and extracted source code
- Identified unsafe SQL query construction
- Exploited SQL injection to bypass policy validation
- Attached `AdministratorAccess` to the original IAM user
- Retrieved secret from AWS Secrets Manager

**Tools Used**
- AWS CLI  
- AWS Lambda  
- IAM & STS  
- AWS Secrets Manager  
- Python  
- SQLite  

**Vulnerabilities Identified**
- SQL injection in Lambda handler
- User-controlled input influencing IAM privilege changes
- Overly permissive IAM policies with wildcards
- Lambda role allowed to attach IAM policies

**Key Lessons**
- Serverless environments are still vulnerable to classic injection flaws
- IAM wildcard permissions enable silent privilege escalation
- Lambda functions must never control IAM privilege assignment
- Temporary STS credentials can be high risk if roles are misconfigured

**Defensive Takeaways**
- Use parameterized queries and strict input validation
- Enforce least-privilege IAM policies
- Restrict `sts:AssumeRole` to exact ARNs
- Monitor IAM and STS activity with CloudTrail and GuardDuty

**Lab Report**
- [⬇️ Download PDF](/assets/reports/CloudGoat-Vulnerable-Lambda.pdf)
- [👁 View in new tab](/assets/reports/CloudGoat-Vulnerable-Lambda.pdf){:target="_blank"}

---

### AWS S3 Enumeration & Credential Escalation
Category: Cloud Security · AWS
Focus: S3 Misconfiguration, IAM Abuse, Credential Exposure

**Problem Statement**
Investigate a static website discovered during a phishing incident and assess whether underlying cloud infrastructure exposes security weaknesses. The objective was to enumerate AWS S3 resources, identify misconfigurations, and determine whether exposed assets could lead to unauthorized access or privilege escalation.

**Approach**
- Analyzed application errors and website source code to identify backend cloud resources.
- Enumerated a publicly accessible S3 bucket using unauthenticated AWS CLI requests.
- Discovered exposed migration artifacts containing hardcoded AWS credentials.
- Pivoted through multiple IAM identities using leaked credentials.
- Escalated privileges to an administrative IAM user.
- Accessed sensitive customer data stored insecurely in the same S3 bucket.
- Analyzed bucket policies to understand inconsistent access behavior between browsers and AWS CLI.

**Tools & Technologies**
- AWS CLI
- Amazon S3
- IAM & STS
- PowerShell (credential analysis)
- Linux command line

**Key Findings**
- Public S3 bucket used for multiple purposes, including static hosting and sensitive internal storage.
- Hardcoded AWS credentials embedded in migration scripts.
- XML export from a Privileged Access Management (PAM) system exposed additional high-privilege credentials.
Weak bucket policy logic allowed enumeration via AWS CLI but blocked browser access.
- Cleartext storage of customer PII and financial data.

**Key Lessons Learned**
- Static website buckets must never store sensitive internal files.
- IAM credentials should never be hardcoded in scripts or configuration files.
- Least-privilege IAM enforcement is critical to preventing lateral movement.
- Secrets should be stored securely using services like AWS Secrets Manager.
- Public access—even briefly—can lead to automated compromise.

**Lab Report**
- [⬇️ Download PDF](/assets/reports/AWS-S3-Enumeration-Basics.pdf)
- [👁 View in new tab](/assets/reports/AWS-S3-Enumeration-Basics.pdf){:target="_blank"}

---



---

### 🛡 Azure Monitor, Defender & Sentinel

**Problem Statement**  
Implement proactive cloud defense and detection.

**Approach**
- Enabled Azure Monitor
- Configured Defender for Cloud
- Built Sentinel analytics rules
- Enabled Just-In-Time VM access

**Tools Used**
Azure Monitor · Defender · Sentinel

**Key Lessons**
- Visibility + response = real security

📄 **Lab Report**
<iframe src="/assets/reports/Azure-Monitor-Defender-Sentinel.pdf" width="100%" height="600px"></iframe>

---

### 🌐 Site-to-Site IPsec VPN

**Problem Statement**  
Secure network-to-network communication.

**Approach**
- Configured ISAKMP & IPsec
- Validated encryption and authentication

**Tools Used**
Cisco Packet Tracer

**Key Lessons**
- Encryption without policy is meaningless

📄 **Lab Report**
<iframe src="/assets/reports/Site-to-Site-IPsec-VPN.pdf" width="100%" height="600px"></iframe>
