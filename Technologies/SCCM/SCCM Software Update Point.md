---
tags:
  - Technologies
  - SCCM
date: 2025-16-10
aliases: ["SUP", "Software Update Point", "WSUS Role", "SCCM SUP"]
---
The **Software Update Point (SUP)** integrates SCCM with **Windows Server Update Services (WSUS)** to manage and deliver updates across an enterprise. It enables centralized control of patching workflows via SCCM.
## Responsibilities

- Synchronizes updates from Microsoft Update (or offline WSUS)
- Allows admins to:
  - Approve updates
  - Assign updates to collections
  - Schedule deployments
- Works with the Management Point to deliver update policy to clients
- Supports both Windows and third-party updates
## Architecture

- SUP is installed as a role within SCCM
- Requires an existing **WSUS instance**
- Can be installed on the **Site Server** or standalone

SUP → WSUS → Microsoft Update
## Update Flow

1. **Sync**  
   - SUP syncs catalog metadata from Microsoft or upstream WSUS

2. **Approval & Assignment**  
   - Admin selects which updates to deploy

3. **Policy Delivery**  
   - Update deployment policy sent to clients via Management Point

4. **Client Action**  
   - Client scans against WSUS
   - Downloads content from Distribution Point
## Logs of Interest

| File | Component | Notes |
|------|-----------|-------|
| `wsyncmgr.log` | SUP | WSUS sync progress |
| `wcm.log` | SUP | WSUS configuration |
| `wuahandler.log` | Client | Client update scan and install |
| `UpdatesDeployment.log` | Client | Deployment enforcement |
| `UpdateStore.log` | Client | Local update store activity |
## Security Considerations

- WSUS can be abused for **malicious update injection** in insecure configurations
- Clients typically **trust update metadata** delivered via SCCM
- SUP compromise can allow:
  - Disruption of patching
  - Update denial
  - Third-party software manipulation (if configured)

---

![[readmore.png]]

- [[SCCM Distribution Point]]
- [[SCCM Credential Storage]]
- [[SCCM Management Point]]
