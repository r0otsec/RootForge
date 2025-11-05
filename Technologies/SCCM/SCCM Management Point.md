---
tags:
  - Technologies
  - SCCM
date: 2025-16-10
aliases: ["MP", "Mgmt Point", "SCCM MP", "Management Point"]
---
The **Management Point (MP)** is the interface between SCCM clients and the core infrastructure. It provides clients with policy and content location data, and receives inventory and status messages in return.
## Responsibilities

- Distributes:
  - Task Sequence policy
  - Application deployment policy
  - Update policy
- Receives:
  - Hardware/software inventory
  - Execution status
  - Client registration
- Provides the location of:
  - Distribution Points
  - SUP
  - Content resources
## Communication Path

- Clients communicate via HTTP or HTTPS (port 80 or 443)
- Authentication is typically **Kerberos**
- Policies are delivered as **XML payloads**
- MPs may be load-balanced or assigned via boundary groups
## Trust Relationships

- Clients trust the MP to provide legitimate policy
- MPs often impersonate **service accounts** during communication
- MP may reside on the same host as the Site Server or be standalone
## Logs of Interest (Client & Server)

| File | Location | Notes |
|------|----------|-------|
| `PolicyAgent.log` | Client | Policy retrieval status |
| `CCMExec.log` | Client | Client operations |
| `mpmsi.log` | MP | Installation of MP role |
| `mpcontrol.log` | MP | MP status and role health |
| `mpclient.log` | MP | Client interaction logs |
## Security Considerations

- Policies delivered by the MP may include **task sequence variables**, sometimes with credentials
- MP compromise = ability to **manipulate policy**, redirect clients, or intercept TS activity
- Can be targeted for **credential exposure**, or to stage **lateral movement** via rogue deployments

---

![[readmore.png]]

- [[SCCM Client]]
- [[SCCM Task Sequences & Deployment]]
- [[SCCM Credential Storage]]
- [[SCCM Distribution Point]]
