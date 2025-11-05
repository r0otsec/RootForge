---
tags:
  - Technologies
  - SCCM
date: 2025-16-10
aliases: ["Site Database", "SQL Database", "SCCM SQL", "SCCM DB", "Site DB"]
---
The SCCM Site Database is hosted on **Microsoft SQL Server** and contains **all operational and configuration data** for the SCCM environment. It is the central backend accessed by the Site Server and other roles.

Compromising the Site Database typically grants **full insight and control** over SCCM infrastructure.
## What’s Stored

The database includes data such as:

- Client inventory (hardware/software)
- Collection membership
- Task Sequences
- Application/package definitions
- Deployment policies
- Account and credential references
- Boundary groups and discovery results
- Status messages and logs
## Key Tables & Views

| View / Table | Description |
|--------------|-------------|
| `v_R_System` | Discovered systems with domain, name, MAC, IP |
| `v_Collection` | Lists SCCM collections |
| `v_Advertisement` | Software deployment info |
| `v_TaskSequencePackage` | Metadata about task sequences |
| `vSMS_R_User` | User discovery info |
| `v_Account` / `vServiceWindow` | Can contain credential references |
| `v_ClientSettingsAssignments` | Lists custom client settings |
| `v_PackageStatusDistPointsSumm` | Content distribution tracking |
| `v_SoftwareUpdate` | Update metadata if SUP is used |

These views can be queried using T-SQL or tools like PowerShell.
## Access Control

- SQL Server often runs **locally on the Site Server**
- SCCM uses a dedicated service account or local system context
- If a user has **local admin on the Site Server**, they can often access SQL without further privilege escalation
## Security Implications

- Many deployments use **weak SQL permissions**
- Credential references may be stored in base64 or even plaintext
- A compromised SCCM operator account may have **read access** to sensitive views
- Task sequence variables and deployment metadata are stored here
## Query Examples

>[!information] Systems in a Collection

```sql
SELECT Name0, ResourceId
FROM v_R_System
WHERE ResourceId IN (
  SELECT ResourceID FROM v_FullCollectionMembership
  WHERE CollectionID = 'XYZ00001'
);
```

>[!information] Installed Software

```sql
SELECT * FROM v_Add_Remove_Programs;
```

>[!information] Task Sequence Info

```sql
SELECT PackageID, Name, Description
FROM v_TaskSequencePackage;
```

---

![[readmore.png]]

- [[SCCM Site Server]]
- [[SCCM Credential Storage]]
- [[SCCM Task Sequences & Deployment]]
