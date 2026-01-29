---
title: "CloudGoat – IAM Privilege Escalation by Policy Rollback"
layout: single
author_profile: true
toc: true
toc_sticky: true
---
## 🧪 Overview

**Category:** Cloud Security · AWS IAM  
**Difficulty:** Intermediate  

### 📌 Problem Statement
Evaluate the security of an AWS IAM environment where a low-privileged user has permissions to manage IAM policy versions.

### 🔍 Attack Path
- Enumerated IAM policies attached to the user
- Identified multiple policy versions
- Discovered `iam:SetDefaultPolicyVersion`
- Rolled back to an overly permissive version
- Escalated privileges to Administrator

### 🛠 Tools Used
- AWS CLI
- AWS IAM
- CloudGoat
- Kali Linux

### 🧠 Key Lessons
- IAM privilege escalation can occur without infrastructure exploits
- Policy versioning is a hidden attack surface
- Least privilege must include version governance

### 🏁 Outcome
Achieved full administrative access by abusing IAM policy rollback.

**Lab Report**
- [⬇️ Download PDF](/assets/reports/CloudGoat-IAM-privilege-escalation-by-rollback-scenario.pdf)
- [👁 View in new tab](/assets/reports/CloudGoat-IAM-privilege-escalation-by-rollback-scenario.pdf){:target="_blank"}