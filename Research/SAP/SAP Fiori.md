---
tags:
 - SAP
---

- UI/UX framework.
- Provides modern web interface for SAP applications - replaces [[SAP GUI]].
- Designed to improve experience across apps (especially [[S4 HANA]]).
- 3 ways to launch it:
	- via URL in the browser.
	- saving URL in SAP GUI under favourites.
	- using transaction code `/UI2/FLP` in SAP GUI.
# Architecture

- `Frontend` - Built with SAPUI5 (JS UI framework).
- `Backend` - typically connects to [[SAP Gateway]] to expose OData services like S4 HANA.
- Deployed via SAP Fiori launchpad.

![[Fiori.png]]
# Launchpad

- Home page is referred to as `Fiori Launchpad`.
- Hosts all available mini apps for departments.
- Each tile represents a separate business application.
	- tiles available are dependent on logged-in users role.
	- tiles also contain dashboards and indicate statuses.
- Contains ability to search across everything for certain keywords - i.e. `Germany`.