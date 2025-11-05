---
tags:
 - SAP
---

- SAP's proprietary communication protocol.
- Used by [[SAP GUI]] to talk directly to the SAP server [[ABAP Stack]].
- Handles screen rendering, user inputs, and data transport between client and backend.
# What It Does

- User runs a T-Code like `VA01` in SAP GUI.
- DIAG sends the actions (button click, field entries) to SAP system.
- Also receives updated screen from the backend.
# Example Workflow

- User opens SAP GUI and runs T-Code `ME21N` (Create Purchase Order).
- DIAG sends a message to the ABAP stack.
- SAP backend processes the request, builds a new screen layout.
- DIAG sends back a "screen update" to SAP GUI.
- SAP GUI redraws the interface with updated fields.
