---
title: "Azure Key Vault & Always Encrypted (SQL)"
layout: single
permalink: /labs/azure-key-vault-always-encrypted/
author_profile: true
toc: true
toc_sticky: true
---

## Overview

**Platform:** Microsoft Azure  
**Focus:** Data Protection, Key Management, Encryption-in-Use  
**Services:** Azure Key Vault, Azure SQL Database, Microsoft Entra ID  
**Difficulty:** Intermediate  

This lab demonstrates the implementation of **Always Encrypted** in Azure SQL Database with **encryption keys and secrets securely managed by Azure Key Vault**. The solution ensures sensitive data remains encrypted **at rest, in transit, and during query execution**, with decryption occurring only in trusted client applications.

---

## Architecture Summary

- Azure SQL Database with Always Encrypted enabled
- Azure Key Vault storing:
  - Column Master Key (CMK)
  - SQL authentication secret
- Client application registered in **Microsoft Entra ID**
- Console application accessing encrypted data using Key Vault–backed keys

---

## Security Objectives

- Prevent plaintext access to sensitive columns (SSN, Birthdate)
- Centralize cryptographic key management
- Eliminate hardcoded secrets
- Enforce identity-based access via Entra ID

---

## Implementation Highlights

### 🔐 Azure Key Vault
- Created a dedicated Key Vault with access policies
- Generated an RSA software-protected encryption key
- Stored SQL authentication credentials as a secret
- Granted controlled access to:
  - User identity
  - Registered application (service principal)

### 🗄 Azure SQL Database (Always Encrypted)
- Created a `Patients` table containing PII
- Encrypted columns:
  - **SSN** → Deterministic encryption (queryable)
  - **BirthDate** → Randomized encryption (non-queryable)
- Column Master Key backed by Azure Key Vault
- Column Encryption Key generated automatically

### 🆔 Application Identity
- Registered a client application in Microsoft Entra ID
- Used client ID and secret for Key Vault authentication
- Enforced least-privilege access to cryptographic operations

### 🧪 Client Application Validation
- Built a .NET console application
- Retrieved encryption keys dynamically from Key Vault
- Inserted and queried encrypted data
- Verified:
  - Encrypted values in SSMS
  - Decrypted values only visible via authorized client

---

## Results

| Scenario | Outcome |
|--------|--------|
Viewing data via SSMS | Encrypted ciphertext |
Querying via app | Decrypted plaintext |
Key exposure | None |
Secrets in code | None |

---

## Key Security Takeaways

- **Always Encrypted** protects data even from DB admins
- **Azure Key Vault** prevents key exfiltration**
- Deterministic vs Randomized encryption impacts queryability
- Identity-based access control is critical for cryptographic assets
- Secrets should never be embedded in application code

---

## Artifacts

- ARM templates for infrastructure deployment
- Encrypted SQL schema
- Key Vault–integrated .NET client application

---

## Conclusion

This lab validates a real-world secure data architecture using Azure-native services. By integrating Azure Key Vault with Always Encrypted, sensitive data remains protected throughout its lifecycle while maintaining application functionality. The solution reflects enterprise-grade security practices aligned with zero-trust and least-privilege principles.

# Full Technical Report

📄 **Detailed Step-by-Step Lab Report**  
- [⬇️ Download PDF](/assets/reports/Azure-KeyVault-Always-Encrypted-Full.pdf)
- [👁 View in new tab](/assets/reports/Azure-KeyVault-Always-Encrypted-Full.pdf){:target="_blank"}