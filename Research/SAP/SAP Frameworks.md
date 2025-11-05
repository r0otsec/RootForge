---
tags:
 - SAP
---
- [[SAP Netweaver]] / [[S4 HANA]] is a framework where SAP is built in.
	- located in the application layer.
- In charge of how the SAP system is going to operate with the OS (how to connect/interface) and how the SAP system connects to the database.
- Framework is service-oriented - divided into two types:
	- `ABAP`
		- modern/most important.
		- where the money is.
		- credit cards, payments, IBANS, etc...
		- mainly utilized as a backend.
	- `J2EE`
		- Java-based.
		- being deprecated.
		- mainly used for extranets or providing web apps.

- [[ABAP Stack|ABAP]]/Java will have own set of services.
	- some shared, some not.
	- each service will have a specific port.

![[SAP Diagram.png]]

