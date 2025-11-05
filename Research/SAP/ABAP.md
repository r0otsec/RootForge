---
tags:
 - SAP
---


- SAP's proprietary programming language.
- Used for custom apps within SAP systems.
- Widely used in [[SAP ECC]], [[S4 HANA]] and other ABAP environments.
- Runs server-side on the [[ABAP Stack]].
# Notes

- ABAP code executes in the application layer (not user device).
- Often confused with the ABAP stack (runtime environment).
# Code Examples

A basic ABAP example to print text to the screen is:

```abap
WRITE 'hello world!'.
```

To declare variables and perform maths - this adds two integers and prints result:

```abap
DATA: lv_num1 TYPE i VALUE 5,
      lv_num2 TYPE i VALUE 3,
      lv_result TYPE i.

lv_result = lv_num1 + lv_num2.

WRITE: / 'Result is:', lv_result.
```

More SAP specific example may be reading customer data from a table title `KNA1`:

```abap
DATA: ls_customer TYPE kna1.

SELECT SINGLE * FROM kna1 INTO ls_customer
  WHERE kunnr = '100000'.

WRITE: / 'Customer:', ls_customer-name1,
       / 'City:', ls_customer-ort01,
       / 'Country:', ls_customer-land1.
```

For context:

- `KNA1` is a standard SAP table storing customer master data.
- The code retrieves one customer with the customer number `100000` and displays name, city and country.




