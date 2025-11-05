---
tags:
 - SAP
---

- Thick client desktop app to interact with SAP backend systems
	- particular for [[ABAP Stack]] focused systems ([[SAP ECC]]/[[S4 HANA]])
- Provides access to SAP transactions, configs, data entry and dev tools.
- Acts as a frontend for managing business processes.
- Communicates with the ABAP Stack.
- Does NOT connect to the database - goes through ABAP layer.
- Gradually being replaced by [[SAP Fiori]].
- 3 different types:
	- SAP GUI for HTML
	- SAP GUI for Windows (most common)
	- SAP GUI for Java

![[SAPGUI.png]]
# Use Cases

Some common use cases include:

- Executing transaction codes.
- Accessing master data.
- Running reports.
- Performing system config.
- Developing ABAP code.
- Managing custom programs.
# Notes

- SAP GUI does not use:
	- SAP Gateway
	- OData
	- HTTP/REST
- It uses:
	- [[DIAG]]
	- [[SAP RFC (Remote Function Call)]] for backend comms.
	- Direct connection to ABAP Stack.