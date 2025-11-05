---
tags:
  - Technologies
  - SCCM
date: 2025-16-10
aliases: ["DP", "Dist Point", "SCCM DP", "Distribution Point"]
---
The **Distribution Point (DP)** is responsible for **serving content** to SCCM clients. This includes application installers, packages, task sequence content, OS images, scripts, and more.

DPs are typically deployed across various sites/subnets to optimize bandwidth and performance.
## Responsibilities

- Hosts:
  - Application content (`.exe`, `.msi`, `.bat`, etc.)
  - Task sequence resources (WIM files, drivers, scripts)
  - Boot images for PXE deployments
- Serves content to:
  - SCCM Clients
  - OSD systems (via PXE)
- May handle PXE boot requests if the PXE role is enabled
## Trust & Access Model

- DPs **impersonate service accounts** to read content from shared folders
- Clients **implicitly trust** the content served by a DP
- Frequently accessible over SMB, HTTP, or HTTPS
- SCCM clients receive DP location info via the Management Point
## PXE Boot Integration

If PXE is enabled on the DP:
- Acts as a PXE responder alongside or instead of WDS
- Provides boot images and BCD files for bare-metal deployments
- Stores PXE content in ` C:\RemoteInstall\SMSImages\<PackageID>\`
## Logs of Interest

| File | Location | Notes |
|------|----------|-------|
| `distmgr.log` | Site Server | Tracks package distribution to DPs |
| `smsdpmon.log` | DP | Distribution Point monitoring |
| `smspxe.log` | DP | PXE boot service activity |
| `pulldp.log` | DP (if Pull DP role) | Logs pull DP file transfer |
| `contenttransfermanager.log` | Client | Logs content download from DP |
## Security Considerations

- Content is often served **anonymously or using basic auth**
- **Executable content can be replaced** on misconfigured or poorly secured DPs
- DPs are commonly accessible from client networks and rarely monitored
- Useful for:
  - Hosting payloads
  - Content redirection
  - PXE spoofing

---

![[readmore.png]]

- [[SCCM PXE Boot & OS Deployment]]
- [[SCCM Task Sequences & Deployment]]
- [[SCCM Credential Storage]]
