---
title: "Azure Role Based Access Control"
layout: single
permalink: /labs/azure-role-based-access-control/
author_profile: true
toc: true
toc_sticky: true
---
# Overview
This lab demonstrates Role-Based Access Control (RBAC) in Azure using Microsoft Entra ID (Azure AD). The goal was to implement group-based access management by creating users and security groups, assigning Azure roles at the resource group scope, and validating permissions. The lab emphasizes scalability, least privilege, and identity governance best practices.

# Lab Scenario
An organization required a proof of concept showing how access is managed through roles assigned to groups instead of individual users. The environment included three administrative tiers:
- Senior Admins
- Junior Admins
- Service Desk

Each group was populated with a specific user and managed using different administrative tools.

# Architecture & Design
- Microsoft Entra ID (Azure AD) for identity management
- Security groups used as RBAC principals
- Azure Resource Group used as the RBAC scope
- Virtual Machine Contributor role assigned to the Service Desk group

# Implementation Summary
- Created users and groups using three management interfaces:
    1. Azure Portal (Senior Admins)
    2. PowerShell (Junior Admins)
    3. Azure CLI (Service Desk)
- Added users to their respective security groups
- Assigned Virtual Machine Contributor role to the Service Desk group at the resource group level
- Verified effective permissions using Access Control (IAM) and access checks

# Security Controls Implemented
1. **Group-based RBAC**
    - Permissions assigned to groups rather than individuals
2. **Least Privilege Access**
    - Service Desk users can manage virtual machines but not networking or storage
3. **Scoped Role Assignment**
    - Role limited to a specific resource group
4. **Auditable Access**
    - Role assignments verified through IAM access checks

# Validation & Testing
- Confirmed group membership for all users
- Verified RBAC role inheritance for Service Desk users
- Ensured Senior and Junior Admins did not inherit unintended permissions
- Demonstrated consistent outcomes across Portal, PowerShell, and CLI workflows

# Key Takeaways
1. RBAC scales better when roles are assigned to groups
2. Microsoft Entra ID centralizes identity and access management
3. Multiple admin tools can manage the same identity objects
4. Least-privilege role assignments reduce blast radius

# Tools & Technologies
1. Microsoft Entra ID (Azure AD)
2. Azure RBAC
3. Azure Portal
4. Azure PowerShell
5. Azure CLI

# Full Technical Report
📄 **Detailed Step-by-Step Lab Report**  
- [⬇️ Download PDF](/assets/reports/Azure-Role-Based-Access-Control.pdf)
- [👁 View in new tab](/assets/reports/Azure-Role-Based-Access-Control.pdf){:target="_blank"}