---
title: "CloudGoat – Vulnerable Lambda"
layout: single
author_profile: true
toc: true
toc_sticky: true
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