---
tags:
 - SAP
---
- A logical division created by SAP.
- Contains at least three different SAP systems of the same "flavour" (convention):
	- `DEV`
	- `QA`
	- `PRODUCTION`
- Company can decide how many tiers to have:
	- May also have a sandbox inside DEV environment.
	- May have a UAT environment between QA and PRODUCTION.

![[SAPPipeline.png]]

- Only one technical enforcement:
	- two SAP systems with the same SID cannot co-exist with the same landscape.
- Each system has a `SID`:
	- three digit name assigned to SAP systems (e.g. 100, 200, etc..)
	- cannot be repeated across the landscape.

