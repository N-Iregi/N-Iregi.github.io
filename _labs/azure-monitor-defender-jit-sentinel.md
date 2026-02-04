---
title: "Azure Monitor, Microsoft Defender for cloud, Enable Just-In Time Access in VMs, Microsoft Sentinel"
layout: single
permalink: /labs/azure-monitor-defender-jit-sentinel/
author_profile: true
toc: true
toc_sticky: true
---
# Objective:
Implement a comprehensive Azure security and monitoring solution to enhance visibility, reduce attack surfaces, and automate threat detection and response across virtual machines (VMs) handling sensitive workloads.

# Key Components & Implementation:

**Azure Monitor**

- Collected and aggregated logs and performance metrics from VMs using Azure Monitor Agent (AMA) and Data Collection Rules (DCRs).

- Created a Log Analytics Workspace and connected storage for centralized telemetry.

- Enabled performance monitoring and troubleshooting dashboards to track application health and availability.

**Microsoft Defender for Cloud**
- Enabled Defender for Servers Plan 2 to protect workloads, including VMs, containers, storage, and databases.

- Continuously assessed security posture, detected misconfigurations, and provided vulnerability insights.

- Integrated AI-based threat detection and DevSecOps workflows for early risk mitigation.

**Just-In-Time (JIT) VM Access**
- Secured management ports (RDP/SSH) by enabling JIT, opening them only temporarily upon approved request.

- Reduced attack surface and prevented unauthorized access or brute-force attempts.

- Configured access requests and logged all JIT activity for auditing.

**Microsoft Sentinel (SIEM)**
- Onboarded Sentinel and connected it to Log Analytics and Azure Activity logs.
- Created analytics rules to detect suspicious activity, including JIT policy deletions.
- Built Logic App playbooks to automate incident response and severity changes.
- Monitored incidents, verified alerts, and ensured effective response workflows.

# Results & Skills Demonstrated:
- Successfully deployed and secured VMs with encryption at host enabled.
- Centralized monitoring and log collection for cloud and hybrid workloads.
- Applied threat detection, analytics, and automated response using Sentinel.
- Strengthened understanding of cloud security operations, SIEM, and proactive threat management.

# Technologies Used:
Azure Monitor · Log Analytics · Microsoft Defender for Cloud · JIT VM Access · Microsoft Sentinel · Azure Logic Apps · KQL · Azure VM · NSGs

# Conclusion:
These labs provided hands-on experience integrating Azure security services to monitor, detect, and respond to threats effectively. They highlighted the synergy between Azure Monitor, Defender for Cloud, JIT access, and Sentinel, demonstrating real-world cloud security operations and SIEM implementation.

# Full Technical Report

📄 **Detailed Step-by-Step Lab Report**  
- [⬇️ Download PDF](/assets/reports/azure-monitor-defender-jit-sentinel.pdf)
- [👁 View in new tab](/assets/reports/azure-monitor-defender-jit-sentinel.pdf){:target="_blank"}