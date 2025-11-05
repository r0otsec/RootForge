---
tags:
  - Technologies
  - SCCM
date: 2025-16-10
aliases: ["Task Sequences", "TS", "SCCM Deployment Workflow", "TS Execution"]
---
Task Sequences (TS) in SCCM are used to automate complex operations like **Operating System Deployment (OSD)**. They define a series of steps that SCCM clients execute in sequence including partitioning disks, applying images, joining domains, installing drivers, and more.
## Where Task Sequences Are Used

- Bare-metal deployments
- PXE boot scenarios
- Refresh/reinstall workflows
- Autopilot-like automation
## How It Works (Simplified)

1. **Collection Targeting**  
   - TS is deployed to a collection of systems (static or dynamic)

2. **Policy Delivery**  
   - Management Point provides policy info to the client
   - Includes TS XML and variable data

3. **Execution**  
   - SCCM client receives and begins executing the sequence
   - Steps run in local `SYSTEM` context
   - Variables and credentials may be used mid-sequence

4. **Reporting & Logging**  
   - Client reports status back to MP/Site Server
   - Logs are generated on the client and server
## Storage and Structure

- TS metadata is stored in the **SQL database**
- Delivered to clients as **XML-based policy**
- Client-side copy may exist in:
  - `C:\_SMSTaskSequence`
  - `C:\Windows\CCM\Cache`
  - `C:\Windows\CCM\Logs\smsts.log`
## Common Use Cases

- Image deployment (.WIM)
- Driver injection
- Domain join
- Application installs
- Post-deploy scripts (e.g., config, firewall, registry)
## Security Implications

- TS may contain **plaintext credentials**, especially:
  - Domain join accounts
  - NAA credentials
  - Share mappings
- Variables passed to steps may expose sensitive config
- Misconfigured or reused task sequences = security drift
## Logs of Interest

- `smsts.log` — primary TS execution log on client
- `StatusAgent.log`, `ExecMgr.log` — monitor actions and return codes
- `MP_ClientIDManager.log` — helps trace TS origin

---

![[readmore.png]]

- [[SCCM Collections and Variables]]
- [[SCCM PXE Boot & OS Deployment]]
- [[SCCM Network Access Accounts (NAA)]]
- [[SCCM Credential Storage]]
