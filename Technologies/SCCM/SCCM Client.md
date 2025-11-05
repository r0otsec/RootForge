---
tags:
  - Technologies
  - SCCM
date: 2025-16-10
aliases: ["SCCM Endpoint Agent", "Client Agent", "Configuration Manager Client"]
---
The SCCM Client is installed on all managed endpoints in an environment. It enables communication between the client system and SCCM infrastructure, enforces policy, reports system state, and executes deployments.

It runs as a Windows service with **SYSTEM-level permissions**.
## Responsibilities

- Pull and apply policy from Management Point (MP)
- Execute Task Sequences
- Report hardware/software inventory
- Run scheduled tasks (e.g., updates, scripts)
- Handle application deployments
## Key Characteristics

- Runs under `NT AUTHORITY\SYSTEM`
- Communicates with:
  - Management Point (for policy, state)
  - Distribution Point (for content)
- Communicates via HTTP/HTTPS (port 80/443 or custom)
## WMI Namespaces

The SCCM Client stores extensive data in WMI:

| Namespace | Purpose |
|-----------|---------|
| `root\ccm` | Core client config, policies |
| `root\ccm\policy\machine\actualconfig` | Task sequence variables, policies |
| `root\ccm\softmgmtagent` | Software deployments |
| `root\cimv2\sms` | Software inventory, execution status |

These WMI locations are key for **recon**, **variable extraction**, and **policy inspection**.
## Common Log Files

| Path | Description |
|------|-------------|
| `C:\Windows\CCM\Logs\smsts.log` | Task sequence execution |
| `C:\Windows\CCM\Logs\ExecMgr.log` | Application execution |
| `C:\Windows\CCM\Logs\PolicyAgent.log` | Policy download |
| `C:\Windows\CCM\Logs\ClientIDManagerStartup.log` | Client registration |
| `C:\Windows\CCM\Logs\CCMSetup.log` | Initial install |
## Client-Side Credential Exposure

Under certain conditions, the client may store or expose credentials:

- In `smsts.log` during task sequences
- In TS XML files in `C:\_SMSTaskSequence`
- In WMI variables (if exposed via policy)

These exposures typically involve:

- NAA accounts
- Domain join accounts
- File share mappings

---

![[readmore.png]]

- [[SCCM Management Point]]
- [[SCCM Task Sequences & Deployment]]
- [[SCCM Credential Storage]]
