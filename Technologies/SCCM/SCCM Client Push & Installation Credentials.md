---
tags:
  - Technologies
  - SCCM
date: 2025-16-10
aliases: ["Client Push", "Client Push Credentials", "SCCM Client Installation"]
---
SCCM allows administrators to push the client agent to remote systems. This is known as **Client Push**. It’s typically triggered from the Site Server and requires **admin-level access** to the target machine.
## How Client Push Works

- Triggered manually or automatically when a system is discovered
- Uses **Windows Management Instrumentation (WMI)** and **Admin$ share**
- Requires:
  - File copy access to `\\<target>\admin$`
  - Ability to execute remote service creation via WMI
## Client Push Account(s)

To perform a client push, SCCM uses one or more **Client Push Installation Accounts**, defined in the console.

These accounts must have:

- **Local administrator rights** on the target system
- **Permission to copy files**, write to the registry, and create services

Multiple accounts can be specified. SCCM will attempt them in order.
## Where Credentials Are Stored

- Stored in the **SCCM Site Database**
- Encrypted, but retrievable from the console by authorized users
- May also appear in:
  - Logs on the Site Server (`ccm.log`)
  - Task sequences if reused for domain join or mapping shares
  - Deployment policies when exported
## Security Considerations

- These accounts are often **domain user accounts** with broad local admin rights
- Compromise of a Client Push account = **lateral movement vector**
- Insecure task sequences may reuse these credentials in plaintext
- Auditing of usage and scope is often lacking in large environments

---

![[readmore.png]]

- [[SCCM Credential Storage]]
- [[SCCM Task Sequences & Deployment]]
- [[SCCM Network Access Accounts (NAA)]]