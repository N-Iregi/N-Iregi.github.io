---
title: "Flaws AWS Challenge"
layout: single
author_profile: true
toc: true
toc_sticky: true
---
## Overview

**Category:** Cloud Security  
**Lab Type:** Hands-on Challenge  
**Difficulty:** Intermediate

This lab explores common mistakes in AWS, from misconfigured S3 buckets to leaked credentials, EC2 snapshots, and SecurityAudit permissions.

---

## Level 1 – S3 Bucket Exposure

- **Objective:** Identify an S3 bucket hosting `flaws.cloud`
- **Steps Taken:**
  1. DNS lookup: `dig flaws.cloud`
  2. NSLookup confirmed region: `us-west-2`
  3. Enumerated bucket: `aws s3 ls s3://flaws.cloud/ --no-sign-request`
  4. Accessed secret file: `aws s3 cp s3://flaws.cloud/secret-dd02c7c.html - --profile flaws`
- **Lesson Learned:** Public S3 buckets with “List” permission allow unintended file discovery.

---

## Level 2 – Unauthorized Authenticated Access

- **Objective:** Access subdomain `level2-*.flaws.cloud`
- **Steps Taken:**
  1. Enumerate bucket with authenticated profile:  
     `aws s3 ls s3://level2-*.flaws.cloud --profile <yourprofile>`
  2. Accessed `secret-e4443fc.html` for next level URL
- **Lesson Learned:** Permissions for “Any Authenticated AWS User” can unintentionally expose data.

---

## Level 3 – Leaked Credentials

- Enumerated bucket:  
  `aws s3 ls s3://level3-*.flaws.cloud/ --profile <yourprofile>`  
- Synced entire bucket:  
  `aws s3 --profile <yourprofile> sync s3://level3-*.flaws.cloud/ .`
- Found `.git` repository and `access_keys.txt`
- Created AWS profile with leaked keys and listed buckets:  
  `aws --profile flaws-access s3 ls`
- **Lesson Learned:** Always rotate keys when leaked.

---

## Level 4 – EC2 Snapshot Access

- Used previous level credentials to list snapshots:  
  `aws --profile flaws-access ec2 describe-snapshots --owner-id <account-id> --region us-west-2`
- Mounted snapshot to new EC2, found `/home/ubuntu/setupNginx.sh`
- Retrieved HTTP basic auth credentials for the web server
- **Lesson Learned:** Public EC2 snapshots can leak secrets and credentials.

---

## Level 5 – HTTP Proxy & Metadata Access

- Accessed proxy on EC2:  
  `http://<ec2-ip>/proxy/<target-site>`
- Discovered AWS metadata IP: `169.254.169.254`
- Retrieved IAM role credentials for EC2
- **Lesson Learned:** Protect access to instance metadata; IMDSv2 is recommended.

---

## Level 6 – SecurityAudit Policy Exploration

- Profile: `level6`
- Listed IAM user, attached policies, policy versions
- Identified Lambda function via API Gateway using:  
  `https://<rest-api-id>.execute-api.us-west-2.amazonaws.com/Prod/level6`
- **Lesson Learned:** Read-only policies can still reveal sensitive information.

---

## Key Takeaways

1. **S3 Misconfigurations:** Avoid overly broad “List” or “Any Authenticated User” permissions.  
2. **Secrets Management:** Rotate credentials and restrict `.git` exposure.  
3. **Snapshots:** Restrict EC2/RDS snapshots to trusted accounts only.  
4. **Metadata API:** Protect `169.254.169.254` access.  
5. **IAM Policies:** Even read-only policies can reveal security-relevant information.

**Lab Report**
- [⬇️ Download PDF](/assets/reports/Flaws-AWS.pdf)
- [👁 View in new tab](/assets/reports/Flaws-AWS.pdf){:target="_blank"}