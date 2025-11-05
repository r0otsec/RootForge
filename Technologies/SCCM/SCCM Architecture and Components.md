---
tags:
  - Technologies
  - SCCM
date: 2025-16-10
aliases: ["SCCM Architecture", "SCCM Components", "SCCM Design"]
---

SCCM is made up of modular components that work together to deliver software, configurations, updates, and OS deployments to client systems across a Windows environment.

Each component has a specific role and introduces its own trust and communication paths, many of which become relevant when assessing the attack surface.
## Site Server

- Core of the SCCM hierarchy
- Hosts critical roles and services
- Initiates actions like client push, application deployment, and task sequences
- Communicates directly with the SQL Site Database
## SQL Site Database

- Central data store for SCCM
- Holds configuration, deployment info, task sequences, collections, accounts, etc.
- Queried constantly by the Site Server
- Access here = full visibility and potential control
## Management Point (MP)

- Primary interface between clients and SCCM
- Delivers policy and task sequence data
- Collects inventory and status messages
- Communication typically over HTTP/HTTPS
- Highly active and trusted in the environment
## Distribution Point (DP)

- Hosts and serves application/package content to clients
- Can act as PXE server for OS deployment
- May impersonate the Site Server to deliver content
- Widely deployed across subnets
## Software Update Point (SUP)

- Integrates SCCM with WSUS
- Downloads and delivers Windows updates
- Works with SCCM policies and client targeting
- Optional component, but common in patching workflows
## SCCM Client

- Installed on managed endpoints
- Receives policies, executes deployments, reports to SCCM
- Runs as `SYSTEM`
- Communicates with MPs and DPs
- Local WMI contains client-side state, configs, and job info
## Architecture Diagram

![[SCCM-Architecture.png]]
## Notes

- Components may be installed on the same system or split across multiple servers.
- Large orgs typically deploy multiple MPs and DPs to reduce load.
- Site hierarchy may include primary sites and secondary sites, depending on scale.

---

![[readmore.png]]

- [[SCCM (System Center Configuration Manager)]]
- [[SCCM Client Push & Installation Credentials]]
- [[SCCM Credential Storage]]
- [[SCCM Task Sequences & Deployment]]
- [[SCCM Network Access Accounts (NAA)]]