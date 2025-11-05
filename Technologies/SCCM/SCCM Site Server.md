---
tags:
  - Technologies
  - SCCM
date: 2025-16-10
aliases: ["Site Server", "Primary Site Server", "SCCM Server"]
---
The Site Server is the **core of the SCCM hierarchy**. It hosts all major roles and services responsible for managing, orchestrating, and executing actions across the environment.

It has full visibility and control over clients, applications, task sequences, collections, and system state.
## Responsibilities

- Hosts core roles (e.g., SMS Executive, Site Control Manager)
- Coordinates:
  - Client Push
  - Application deployments
  - Task Sequence execution
  - Site-to-site replication (if applicable)
- Communicates constantly with:
  - SQL Site Database
  - Management Points
  - Distribution Points
## Privileges and Trust

- Has **full access** to the SCCM SQL database
- Often runs under a **high-privileged domain service account**
- Trusted by all SCCM clients and infrastructure
- In many environments, runs on a **domain-joined Windows Server**
## Log Files of Interest

| Path | Description |
|------|-------------|
| `C:\Program Files\Microsoft Configuration Manager\Logs\ccm.log` | Client push actions |
| `smsexec.log` | SMS Executive engine |
| `mpmsi.log`, `hman.log` | Role installation, hierarchy management |
| `sitectrl.log` | Site control and configuration changes |
## Security Considerations

- Site Server compromise = **full SCCM control**
- If colocated with SQL, it often becomes a **single point of takeover**
- Can impersonate client push and deployment accounts
- Highly trusted in the domain - may have GPO and AD visibility

---

![[readmore.png]]

- [[SCCM Architecture and Components]]
- [[SCCM Client Push & Installation Credentials]]
- [[SCCM Site Database (SQL)]]
