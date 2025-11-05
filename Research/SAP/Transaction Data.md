---
tags:
 - SAP
---
- Can be changed
- Created as many apps/users can access the same master data if they have authorization.
- Each event is registered, typically in form of document
- Represents an event which the master data participates.
	- e.g. something happened on the system:
		- sold an item (sales order doc)
		- customer has paid (invoice)
		- item released from stock (material movement doc)
		- item shipped (shipment doc)

![[Master Data.png]]
# Example

A McDonalds receipt contains both transaction data (Order) and master data (material/company code):

![[McDs.png]]