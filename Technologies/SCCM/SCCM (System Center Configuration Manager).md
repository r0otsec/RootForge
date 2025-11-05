---
tags:
  - Technologies
  - SCCM
date: 2025-16-10
aliases: ["SCCM", "System Center Configuration Manager", "Microsoft Endpoint Configuration Manager", "MECM"]
---

## Overview

System Center Configuration Manager (SCCM), formerly Microsoft Endpoint Configuration Manager (MECM), is a systems management platform used to automate software deployment, manage configurations, and patch Windows systems in enterprise environments.

SCCM has broad access over client systems and ties deeply into **Active Directory**, **Windows services**, **SQL Server**, and **PowerShell**. As such, it presents a large and often misunderstood attack surface.
## Core Components

SCCM is composed of several architectural pieces that work together:

- **Site Server**: Central management authority; hosts core services
- **Management Point (MP)**: Client communication and policy distribution
- **Distribution Point (DP)**: Hosts and serves package content
- **Site Database (SQL)**: Stores all SCCM configuration and client data
- **Software Update Point (SUP)**: Integrates with WSUS for patching
- **SCCM Client**: Installed on endpoints to communicate with SCCM infrastructure

Each component has its own attack vectors and trust relationships.
## SCCM Architecture

![[SCCM-Architecture.png]]

## Trust Model

SCCM operates under a **high-trust, centralized model**:

- MPs and DPs **impersonate SCCM service accounts**
- Clients trust Management Points implicitly
- SQL backend contains all system state and credentials
- AD integration means SCCM often runs with **domain-level rights**

Attackers can leverage this model to escalate from local admin on a client → SCCM credentials → full domain compromise.
## Credentials in SCCM

Common credential storage vectors:

| Source         | Notes                                                     |
| -------------- | --------------------------------------------------------- |
| Client Push    | Often contains plaintext domain creds                     |
| Task Sequences | May embed passwords in XML or WMI                         |
| Applications   | May contain install strings with embedded creds           |
| Database       | `v_Accounts` and `vSMS_R_System` tables can leak accounts |

---

![[readmore.png]]

- [[SCCM Architecture and Components]]
- [[SCCM Site Server]]
- [[SCCM Management Point]]
- [[SCCM Distribution Point]]
- [[SCCM Software Update Point]]
- [[SCCM Client]]
- [[SCCM Credential Storage]]