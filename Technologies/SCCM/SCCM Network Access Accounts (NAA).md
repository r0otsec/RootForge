---
tags:
  - Technologies
  - SCCM
date: 2025-16-10
aliases: ["NAA", "Network Access Account", "Network Access Accounts"]
---
A **Network Access Account (NAA)** is used by SCCM clients during operating system deployment (OSD), specifically **before a user logs in or network authentication is available**. It allows access to content on Distribution Points during WinPE or early deployment phases.
## Purpose

The NAA is used when the client is:

- **Not domain joined**
- **Running in WinPE**
- **Without valid Kerberos credentials**

It enables the client to download necessary files from DPs to continue the task sequence.
## Where It’s Used

- During **PXE Boot / OSD**
- When downloading:
  - Boot images
  - Task sequences
  - Application content
## Permissions Required

The NAA must have:

- **Read access** to content libraries on Distribution Points
- Often granted read on shared SCCM content folders (`\\DP\SMSPKGx$`)

In many environments, the NAA is **overprivileged**, or reused across multiple deployment scopes.
## Credential Storage

- Stored in the **SCCM Console**
- Stored in the **SQL Database**
- Can be found in:
  - Deployment policy XML
  - Task Sequence logs (`smsts.log`)
  - WIM cache (`C:\_SMSTaskSequence`)
  - SCCM Client WMI namespaces
## Security Considerations

- **Commonly misconfigured** with excessive privileges
- Frequently reused across multiple task sequences or collections
- Passwords may be stored or transmitted in **plaintext**
- Extraction possible via:
  - SQL queries
  - Policy decryption
  - Client-side logs

---

![[readmore.png]]

- [[SCCM Credential Storage]]
- [[SCCM Task Sequences & Deployment]]
- [[SCCM PXE Boot & OS Deployment]]
