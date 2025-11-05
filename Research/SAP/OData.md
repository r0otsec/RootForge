---
tags:
 - SAP
---

- Web API standard.
- Allows apps ([[SAP Fiori]]) to query and manipulate data over HTTP.
- Language that [[SAP Gateway]] uses to expose SAP data to outside world.
- REST API for databases.
- Used in SAP Gateway, SAP Fiori and 3rd-party apps.
# Real-World Example

A Fiori app may display sales order and call the URL behind the scenes:

```bash
/sap/opu/odata/sap/ZSALES_ORDER_SRV/Orders?$filter=Customer eq '100000'
```

SAP Gateway uses it to run [[ABAP]] code, grab data from `VBAK` and return it as JSON to the app.

- User sees a list of sales order.
- Frontend dev sees a clean API.
- SAP system sees an ABAP call behind an OData service.