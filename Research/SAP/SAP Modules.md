---
tags:
 - SAP
---

- Two types of modules:
	- `Technical modules`
	- `Functional modules`
# Technical Modules

- Not directly replicating business activity.
- Provides support to functional modules.
- Maintain/tune SAP landscape:
	- schedules tasks
	- troubleshoots
	- builds apps
	- downloads/installs updates
	- plans/execute migrations
- Most users will not come into contact with any technical modules.
# Functional Modules

- Support transactions aligned with business processes.
- Strong integration (send info between each other efficiently).
- Orgs choose which ones they need to implement.
- Some common groups/modules include:
	- `Accounting group` - includes financial accounting/controlling
		- generates balance sheets and so on (`financial accounting`).
		- identifies, measures, analyses, interprets, communicates info to managers (`controlling`).
	- `Project System` - manages project through lifecycle, tightly integrated with finance/logistics.
	- `Logistics group` - includes sales distribution, material management, production planning:
		- SD - stores/manages customer/product related data.
		- MM - provides companies with materials and inventory management.
		- PP - material requirements planning, bill of material, routings, etc..
			- revolves around master data stored in centralized master data tables.
				- types include materials, master, work, centre, etc...
				- master data creates transactional data (prod order, sales order, purchase orders).
	- `Quality management` - ensure/improve on quality of products (inspections).
	- `Plant maintenance` - maintain equipment involved in production.
	- `Human capital management` - recruitment, screening, interviewing 

![[Modules.png]]
