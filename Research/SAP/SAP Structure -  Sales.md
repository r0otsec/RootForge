---
tags:
 - SAP
---
- [[SAP]] provides modules to complete SAP sales/distribution organizational structures:
	- sales areas
	- distribution channels
	- divisions
	- etc...
- Consists of 2 steps:
	- creation of organizational elements in SAP system
	- linking each element as per requirements
- `Sales organization` is the highest level
	- responsible for distribution of goods/services
	- recommended to keep number of sales organizations to minimum.
	- each one assigned to a company code
- `Distribution channel`:
	- tells medium why product/services are distributed to end user division.
	- organizational structure which represents a product/service line in a single org.
	- assigned to sales organizations
	- also have an allocation with a plant (e.g. Internet is tied to Rotterdam)
	- can get supplies from several plants
- `Divisions`:
	- products divided into three divisions for example.
# Sales Area

- Known as an entity.
- Required to process an order in the company.
- Comprises:
	- sales organization
	- distribution channel
	- division 

![[Sales.png]]
# Sales Groups

- Makes it possible to group sales assistants who do the same activities.
- Can perform activities for several sales offices.
- Several groups can be operative in one office:
