---
tags:
 - SAP
---
- Requires 3 pieces of information (provided by SAP admin team):
	- client (mandatory)
	- user
	- password
- Client is 3 digits long.
- User accounts are client-dependent.

![[Logon.png]]
# Password Rules

- Various default rules for passwords:
	- can be letters, numbers and special chars.
	- must be between 3 and 40 chars (standard is 6).
	- first char must not be a question mark (`?`).
	- first 3 chars must differ from the username and must not be identical.
	- none of the first 3 can be a blank space.
	- password must not be SAP* or PASS.
	- must differ from previous 5 passwords.
- Admins can reset password that can be used once and must be changed immediately.
# Default Values (SAP Fiori)

- Available under user settings in Fiori
- Contains values to appear when starting new transactions:
	- control areas
	- cost centers
	- cost elements
	- etc..

![[Default-Values.png]]

# Success/Warning/Error Messages

- Can be enabled under SAP GUI Options --> Interaction Design --> Notifications:

![[Notifications.png]]