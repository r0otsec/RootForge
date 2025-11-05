---
tags:
 - SAP
---
# Operating Concern

- Main organizational unit of profitability analysis module:
	- known as `COPA`
- Acts management tools to analyse specific markets and business segments.
- Configuration is client independent:
	- once created under one client, it automatically impacts all of them.
- COPA holds highest rank.
# Controlling Area

- Central unit within controlling module.
- Assigned to operating concern.
- Environment where internal reporting to management is done by managing cost/revenue.
- `Organizational unit` within [[SAP Organizational Structure|SAP structure]] represents closed system used for cost accounting.
# Company Codes

- Controlling areas contain one or more `Company Codes` which operate in different currencies.
- Company code within an area must use the same operational chart of accounts and fiscal year variant.
	- e.g. International enterprise may have subsidiaries in France.
		- cost can be analysed per country with use of controlling area of France.

![[Finance.png]]
# Chart of Accounts

- Variant created at client level.
- Contains the structure and basic information about general ledger accounts.
- After creation, it's assigned to company codes 
- Enterprise structure information to be given is:
	- Language
	- Length of general accounts
	- Number
	- Manual/Automatic creative cost elements group.
	- Chart of accounts etc..
# Cost Center

- OU in controlling area representing delimited location where costs occur.
- One type of hierarchy needs to be created for cost center account.
- Cost center should be assigned and grouped for reporting.
- Cost centers are company code specific:
	- assigned to company codes & enterprise structure.

![[Chart.png]]
# Profit Centres

- Generally defined by organization on basis of responsibility.
- Let you determine profit/loss by profit centre using period accounting/cost of sales approach.
- Management oriented unit.
- International enterprises can analyse costs and revenues per product group:
	- e.g. by creating within the controlling area the net profit centres.

![[Profit.png]]
# Charts of Depreciation

- Highest level of organization structure.
- Holds entire asset accounting relevant settings:
	- e.g. depreciation areas/methods specific to a country
- Usually country specific.
- More than one company code can be assigned to a single chart.
- Depreciation area represents independent depreciation book:
	- different values can be calculated in parallel for each fixed asset.

![[Depreciation.png]]
