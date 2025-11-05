---
tags:
 - SAP
---
- Refers to the ABAP application server
	- part of [[SAP Netweaver]]
- Provides runtime environment for executing programs written in [[ABAP]].
- Core backend layer for many SAP systems - including [[SAP ECC]] and [[S4 HANA]].
# Purpose

- Runs business logic of SAP applications.
- Handles DB interactions, user authorizations, background jobs, transactions.
# Key Components

- `ABAP Runtime Environment` - executes ABAP programs.
- `Dictionary (DDIC)` - manages data definitions and table structures.
- `T-Code Layer` - supports SAP GUI transaction codes.
- `Function Modules / BAPIs` - exposed interfaces for programmatic access.
- `Reports/Forms/Workflows` - built-in tools for business processes.
# Role

- Used in ECC, S4 HANA and other systems.
- Acts as the backend in client-server models:
	- `Frontend` - [[SAP GUI]] or [[SAP Fiori]].
	- `Backend` - ABAP Stack processes logic and connects to database.

