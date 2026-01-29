---
title: "AWS S3 Enumeration Basics"
layout: single
author_profile: true
toc: true
toc_sticky: true
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