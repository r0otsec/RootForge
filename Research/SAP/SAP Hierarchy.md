---
tags:
 - SAP
---
- Number of different levels to understand:
	- `Client`
	- `Company`
	- `Controlling Area`
	- `Operating Concern`
	- `Plant`
	- `Storage Location`
# Client

- Highest organizational level.
- Usually only one client for entire organization/concern.
- Several clients can be used for different orgs using same installation.
- Have separate and unrelated data.
- Each client is detached, individual work environment.
- All customizing activities are related to one client:
	- activity only affects the client where a user is working.
# Company

- Client can contain many companies.
- Company used for subsidiary companies within a concern.
- Each company identified by unique code of 4 characters:
	- intended as an organizationally and legally independent entity
- Company code represents an independent legal entity with fiscal obligations.
- All data relating to flow of funds are linked to the code.
- All separate financial concepts should lead to one balance sheet and one statement.
- Most important data recorded:
	- `Chart of Accounts`
	- `Currency`
	- `Address`

![[Pasted image 20250601211711.png]]
# Controlling Area

- OU used to represent closed system for cost accounting purposes.
- May include single/multiple company codes that can use different currencies.
- Company codes must use the same operative chart of accounts.
- All internal allocations refer exclusively to objects in same controlling area.
# Operating Concern

- Accountability areas within business.
- Used for allocation of costs to cost centres.
- Each cost centre is allocated to an operating concern.
- Each operating concern can represent more than one company.

![[Pasted image 20250601211841.png]]
# Plant

- Businesses can be subdivided into operational facilities called plants:
	- used in the logistic module.
- Many storage locations can be assigned to each plant.
- Plants/business areas are not mutually exclusive.
	- plants can make various products paid for in different business areas.
	- business area can handle products from various plants.
- Plant is concept to which logistical data are linked:
	- allocation from plant to the company code.
	- financial consequences of logistical process can be passed on to financial modules.
- For setting up plant, only address must be entered.
# Storage Location

- Can consist of various storage areas.
- Allow differentiation between various stocks of material in a plant.
- Units are important for the valuation and planning.
- Only a code/specification are recorded.
- No address needed due to assumptions that the storage location is in the same place as the plant.
- Very important for stock registrations, production planning, and requirement planning.

![[Pasted image 20250601212151.png]]