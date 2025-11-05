---
tags:
 - SAP
---
- [[SAP Organizational Structure|Organizational structure]] in MM consists of following levels:
	- `Client`
	- `Company code`
	- `Plant`
	- `Storage location`
	- `Purchase organization`
	- `Purchasing group`
- Levels follow a logic:
	- cannot have a storage location without a plant.

![[MM Structure.png]]
# Client

- commercial organizational unit.
- contains own set of [[Master Data]] with independent table set.
- Highest level in the [[SAP]] system
	- client data maintained is valid for all organizational levels.
# Company Code

- Independent accounting unit.
- Legal entity with own profit/loss and balance sheets.
- Smallest unit where a complete independent account set can be replicated.
- Smallest OU where complete self contained set of accounts can be made for external reporting.
	- e.g. university decides each faculty be depicted in SAP as a company with a company code instead of one code for whole university.
# Plant

- OU within a company where activities take place.
- Produces and makes good available for the company.
- Unit with manufacturing facility, warehouse, distribution centre, regional sales office within logistics.
	- subdivides enterprise into different aspects.
- Various materials are entered in general ledger accounts under account scheme:
	- takes place by control parameters.
# Storage Location

- Unit that differentiates between different material stock in a plant.
- Place where stock is kept physically.
- Plants can have multiple storage locations.
- All data stored at storage location level for particular location.
# Purchasing Organization

- Unit under company or plant.
- Responsible for procurement activities per requirements.
- Responsible for external procurement.
- Can be at a client level (centralized purchasing organization):
	- e.g. company specific/plan specific purchasing group.
# Purchasing Group

- Unit responsible for everyday procurement activities.
- Buyer/group of buyers responsible for procurement activities in purchasing organization.

