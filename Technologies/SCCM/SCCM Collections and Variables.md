---
tags:
  - Technologies
  - SCCM
date: 2025-16-10
aliases: ["Collections", "SCCM Collections", "SCCM Variables", "TS Variables"]
---
Collections are SCCM's way of grouping systems. They're used to **target deployments**, **gather information**, and **apply configurations**. A system can be part of multiple collections.
## Types of Collections

- **Static Collections**
  - Manually populated
  - Useful for targeting specific hosts or small groups

- **Dynamic Collections**
  - Rule-based
  - Evaluated automatically based on criteria (e.g., OS, AD group, hostname pattern)

Collections are stored in the SCCM SQL database, and tied to resources in the `v_R_System` and related views.
## Collection Evaluation

SCCM regularly reevaluates dynamic collections based on their update schedule. This means new systems that meet criteria are automatically added.

Policy targeting (like software deployments) relies on the **membership evaluation result** at the time of execution.
## Variables

Variables can be defined at multiple levels:

- **Collection Variables**
- **Computer Variables**
- **Task Sequence Variables**

They're commonly used during **OS Deployment** and **Task Sequence execution** to define behavior such as:

- Domain join credentials
- Software packages to install
- Device-specific configurations

Variables are stored in **WMI** on the client and **SQL tables** on the server. Task sequence variables may also appear in the client cache (`C:\_SMSTaskSequence`), and sometimes in plaintext.
## Security Implication

While not inherently sensitive, **variables often store credentials**, domain info, and custom deployment logic — especially in Task Sequences. These can be exposed in:

- SQL queries
- WMI
- Policy XML
- Log files on clients (`smsts.log`)

---

![[readmore.png]]

- [[SCCM Task Sequences & Deployment]]
- [[SCCM Credential Storage]]
- [[SCCM PXE Boot & OS Deployment]]
- [[SCCM Client Push & Installation Credentials]]
