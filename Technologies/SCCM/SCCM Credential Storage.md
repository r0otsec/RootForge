---
tags:
  - Technologies
  - SCCM
date: 2025-16-10
aliases: ["Credential Storage", "SCCM Credentials", "SCCM Password Locations"]
---
SCCM stores and uses several types of credentials across its infrastructure. These credentials support deployments, domain joins, network access, and administration — but are often mishandled or exposed through insecure configuration.
## Credential Types in SCCM

| Type | Purpose |
|------|---------|
| **Client Push Account** | Used to install SCCM client agents remotely |
| **Network Access Account (NAA)** | Grants access to content during pre-login OSD phases |
| **Domain Join Account** | Joins machine to Active Directory during deployment |
| **Task Sequence Credentials** | Used in script steps, share access, service installs, etc. |
| **Console User Credentials** | Admin/operator accounts used to manage SCCM |
| **Service Accounts** | Used by SCCM services and scheduled tasks |
## Where Credentials Are Stored or Exposed

### SCCM Console

- Credentials are entered via GUI forms (e.g., NAA, Client Push)
- Stored in the Site Database, retrievable with access
### SQL Database

- Tables such as:
  - `v_Accounts`
  - `vSMS_R_System`
  - `v_TaskSequencePackage`
- May store references to credentials, domain info, and more
### Policy XML

- Task Sequence deployments include variables in XML format
- Policies delivered to clients may contain passwords in plaintext
### WMI (Client Side)

- TS variables stored in namespaces like:
  - `root\ccm\policy\machine\actualconfig`
- Can include usernames, domains, and sometimes passwords
### File System

- `C:\_SMSTaskSequence\` may contain:
  - TS XML files with embedded variables
  - Deployment scripts with hardcoded credentials
- `C:\Windows\CCM\Logs\smsts.log` can reveal variables and account activity
## Example Extraction Paths

| Location | Notes |
|----------|-------|
| SQL Server | Use PowerShell or SQL queries to extract from SCCM views |
| Policy XML | Found in `Policy` WMI or client TS cache |
| Logs | Look for `smsts.log`, `execmgr.log`, `ccmsetup.log` |
| Live Memory | During active TS execution, creds may be present in memory |
## Security Implications

- Credentials are often **overprivileged and reused**
- Admins may not know they’re stored in **client-side logs**
- Task Sequences are a common vector for plaintext leak
- With client access, an attacker can harvest creds **without needing to pop the site server**

---

![[readmore.png]]

- [[SCCM Client Push & Installation Credentials]]
- [[SCCM Network Access Accounts (NAA)]]
- [[SCCM Task Sequences & Deployment]]