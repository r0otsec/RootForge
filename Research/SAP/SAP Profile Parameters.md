---
tags:
 - SAP
---
- Config setting dictating how SAP systems or instances operate.
- More than 2000+ profile parameters on a modern SAP ABAP system.
- Used to configure various options including:
	- path of the logs
	- amount of RAM for VM
	- minimum password length
	- and many more...

>[!danger] Kernel Age
>SAP's kernel is very old - if you change the configuration of profile parameters, the SAP system must be restarted. Restarting a SAP system is very restricted (1 per year/per quarter).
# Syntax

- Format is provided in `key value pairs` such as:
	- `DIR_AUDIT = /usr/sap/<SID>/DVEBMGS00/log/`

