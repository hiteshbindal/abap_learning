# Lecture 3 — Exercises: Control Flow, Methods, and Classes

**Module:** M-SAP-WP-1.2 Client-Server Communication with SAP ABAP  
**Programme:** SAP Engineering and Analytics (M.Sc.)

> **Before you start:** Read `Lecture_03_Theory.md` first. These exercises assume you understand control structures (IF, CASE, DO, WHILE) and ABAP's class and method syntax.

---

## Learning Objectives

After these exercises, you should be able to:

1. Write `IF / ELSEIF / ELSE`, `CASE / WHEN`, and `DO` blocks to branch and repeat.
2. Define and call static methods (`CLASS-METHODS`) within a class.
3. Define instance methods (`METHODS`) and call them on the current object with `me->`.
4. Create a second class as a reusable component and instantiate it with `NEW`.
5. Apply the `PRIVATE` section to hide data from callers.

---

## Reminder: The BTP Class Template

All runnable programs use this skeleton. Copy it for every new class:

```abap
CLASS z_your_class_name DEFINITION PUBLIC FINAL CREATE PUBLIC.
  PUBLIC SECTION.
    INTERFACES if_oo_adt_classrun.
ENDCLASS.

CLASS z_your_class_name IMPLEMENTATION.
  METHOD if_oo_adt_classrun~main.

    " your code here

  ENDMETHOD.
ENDCLASS.
```

Run with **F9**. Activate first with **Ctrl+F3** after every edit.

---

## Exercise 1: Control Flow — Price Categorisation and Purchase Summary ✋

### Goal

Practice `IF / ELSEIF / ELSE`, `CASE / WHEN`, and `DO` in a single class that simulates three separate business checks.

### Create class `Z_CONTROL_FLOW` in your package.

```abap
CLASS z_control_flow DEFINITION PUBLIC FINAL CREATE PUBLIC.
  PUBLIC SECTION.
    INTERFACES if_oo_adt_classrun.
ENDCLASS.

CLASS z_control_flow IMPLEMENTATION.
  METHOD if_oo_adt_classrun~main.
    DATA lv_price      TYPE p LENGTH 8  DECIMALS 2.
    DATA lv_quantity   TYPE i.
    DATA lv_category   TYPE string.
    DATA lv_dept       TYPE c LENGTH 4.
    DATA lv_dept_name  TYPE string.
    DATA lv_total      TYPE p LENGTH 10 DECIMALS 2.

    " ─── Part 1: IF / ELSEIF — price category ───────────────────
    lv_price = '149.99'.

    IF lv_price < 50.
      lv_category = 'Budget'.
    ELSEIF lv_price < 200.
      lv_category = 'Mid-range'.
    ELSEIF lv_price < 1000.
      lv_category = 'Premium'.
    ELSE.
      lv_category = 'Luxury'.
    ENDIF.

    out->write( |Price { lv_price } EUR → Category: { lv_category }| ).

    " ─── Part 2: CASE — department code to full name ─────────────
    lv_dept = 'FIN'.

    CASE lv_dept.
      WHEN 'SALE'.
        lv_dept_name = 'Sales'.
      WHEN 'FIN'.
        lv_dept_name = 'Finance'.
      WHEN 'HR'.
        lv_dept_name = 'Human Resources'.
      WHEN 'IT'.
        lv_dept_name = 'Information Technology'.
      WHEN OTHERS.
        lv_dept_name = 'Unknown Department'.
    ENDCASE.

    out->write( |Department code { lv_dept } → { lv_dept_name }| ).

    " ─── Part 3: DO — running purchase total ─────────────────────
    lv_price    = '25.00'.
    lv_quantity = 5.
    lv_total    = 0.

    out->write( '' ).
    out->write( '--- Purchase Items ---' ).

    DO lv_quantity TIMES.
      lv_total = lv_total + lv_price.
      out->write( |Item { sy-index }: { lv_price } EUR   Running total: { lv_total } EUR| ).
    ENDDO.

    out->write( '' ).
    out->write( |Final total: { lv_total } EUR for { lv_quantity } items.| ).
  ENDMETHOD.
ENDCLASS.
```

Activate (Ctrl+F3) and execute (F9).

### Questions

1. Change `lv_price` to `'49.99'`. Which branch of the `IF` runs? Now set it to `'50.00'`. Which branch runs? Explain the boundary.
2. Change `lv_dept` to `'LOG'`. What output appears? Why?
3. The `DO` loop runs `lv_quantity` times. Set `lv_quantity = 0`. What output does the `DO` block produce?
4. Can you rewrite the price-category `IF / ELSEIF` as a `CASE`? Try it — what limitation stops you?

---

## Exercise 2: Static Methods — Discount Calculator ✋

### Goal

Move calculation logic out of `main` and into static methods. Static methods can be called directly by name inside the same class, with no object required.

### Create class `Z_DISCOUNT_CALCULATOR` in your package.

```abap
CLASS z_discount_calculator DEFINITION PUBLIC FINAL CREATE PUBLIC.
  PUBLIC SECTION.
    INTERFACES if_oo_adt_classrun.

    CLASS-METHODS get_discount_rate
      IMPORTING
        iv_quantity   TYPE i
      RETURNING
        VALUE(rv_rate) TYPE p LENGTH 5 DECIMALS 2.

    CLASS-METHODS calculate_order_total
      IMPORTING
        iv_unit_price TYPE p LENGTH 8  DECIMALS 2
        iv_quantity   TYPE i
      RETURNING
        VALUE(rv_total) TYPE p LENGTH 10 DECIMALS 2.

    CLASS-METHODS classify_customer
      IMPORTING
        iv_annual_spend TYPE p LENGTH 12 DECIMALS 2
      RETURNING
        VALUE(rv_tier)  TYPE string.
ENDCLASS.

CLASS z_discount_calculator IMPLEMENTATION.
  METHOD if_oo_adt_classrun~main.
    DATA lv_rate  TYPE p LENGTH 5  DECIMALS 2.
    DATA lv_total TYPE p LENGTH 10 DECIMALS 2.
    DATA lv_tier  TYPE string.

    " Test discount rates
    lv_rate = get_discount_rate( iv_quantity = 3 ).
    out->write( |Quantity  3 → discount: { lv_rate }%| ).

    lv_rate = get_discount_rate( iv_quantity = 7 ).
    out->write( |Quantity  7 → discount: { lv_rate }%| ).

    lv_rate = get_discount_rate( iv_quantity = 15 ).
    out->write( |Quantity 15 → discount: { lv_rate }%| ).

    out->write( '' ).

    " Test order total with discount included
    lv_total = calculate_order_total( iv_unit_price = '50.00'  iv_quantity = 15 ).
    out->write( |15 × 50.00 EUR with discount: { lv_total } EUR| ).

    out->write( '' ).

    " Test customer tiers
    lv_tier = classify_customer( iv_annual_spend = '800.00' ).
    out->write( |Annual spend     800 EUR → Tier: { lv_tier }| ).

    lv_tier = classify_customer( iv_annual_spend = '5000.00' ).
    out->write( |Annual spend   5,000 EUR → Tier: { lv_tier }| ).

    lv_tier = classify_customer( iv_annual_spend = '25000.00' ).
    out->write( |Annual spend  25,000 EUR → Tier: { lv_tier }| ).
  ENDMETHOD.

  METHOD get_discount_rate.
    IF iv_quantity >= 10.
      rv_rate = '10.00'.
    ELSEIF iv_quantity >= 5.
      rv_rate = '5.00'.
    ELSE.
      rv_rate = '0.00'.
    ENDIF.
  ENDMETHOD.

  METHOD calculate_order_total.
    DATA lv_rate     TYPE p LENGTH 5  DECIMALS 2.
    DATA lv_gross    TYPE p LENGTH 10 DECIMALS 2.
    DATA lv_discount TYPE p LENGTH 10 DECIMALS 2.

    lv_rate     = get_discount_rate( iv_quantity = iv_quantity ).
    lv_gross    = iv_unit_price * iv_quantity.
    lv_discount = lv_gross * lv_rate / 100.
    rv_total    = lv_gross - lv_discount.
  ENDMETHOD.

  METHOD classify_customer.
    IF iv_annual_spend >= 20000.
      rv_tier = 'Platinum'.
    ELSEIF iv_annual_spend >= 5000.
      rv_tier = 'Gold'.
    ELSEIF iv_annual_spend >= 1000.
      rv_tier = 'Silver'.
    ELSE.
      rv_tier = 'Standard'.
    ENDIF.
  ENDMETHOD.
ENDCLASS.
```

### Questions

1. Inside `calculate_order_total`, `get_discount_rate` is called without any prefix. Why does this work? What prefix would you add if calling it from a completely separate class?
2. What would happen if you changed `CLASS-METHODS get_discount_rate` to `METHODS get_discount_rate` (removing `CLASS-`)? Would the call in `calculate_order_total` still compile? Why or why not?
3. Add a fourth test in `main` with `iv_quantity = 5`. What discount rate do you expect based on the logic? Verify.
4. `calculate_order_total` calls `get_discount_rate` internally. What is the advantage of this over duplicating the `IF / ELSEIF` logic inside `calculate_order_total`?

---

## Exercise 3: Instance Methods — Salary Calculator ✋

### Goal

Add instance methods (`METHODS`) to the runnable class and call them using `me->`. Unlike static methods, instance methods can access instance attributes — but in this exercise the methods work purely with their parameters, which is enough to illustrate the `me->` call pattern.

### Create class `Z_SALARY_CALCULATOR` in your package.

```abap
CLASS z_salary_calculator DEFINITION PUBLIC FINAL CREATE PUBLIC.
  PUBLIC SECTION.
    INTERFACES if_oo_adt_classrun.

    METHODS calculate_annual_gross
      IMPORTING
        iv_monthly     TYPE p LENGTH 8  DECIMALS 2
      RETURNING
        VALUE(rv_annual) TYPE p LENGTH 10 DECIMALS 2.

    METHODS get_tax_bracket
      IMPORTING
        iv_annual      TYPE p LENGTH 10 DECIMALS 2
      RETURNING
        VALUE(rv_bracket) TYPE string.

    METHODS calculate_income_tax
      IMPORTING
        iv_annual      TYPE p LENGTH 10 DECIMALS 2
      RETURNING
        VALUE(rv_tax)  TYPE p LENGTH 10 DECIMALS 2.
ENDCLASS.

CLASS z_salary_calculator IMPLEMENTATION.
  METHOD if_oo_adt_classrun~main.
    DATA lv_monthly TYPE p LENGTH 8  DECIMALS 2 VALUE '4500.00'.
    DATA lv_annual  TYPE p LENGTH 10 DECIMALS 2.
    DATA lv_bracket TYPE string.
    DATA lv_tax     TYPE p LENGTH 10 DECIMALS 2.
    DATA lv_net     TYPE p LENGTH 10 DECIMALS 2.

    lv_annual  = me->calculate_annual_gross( iv_monthly = lv_monthly ).
    lv_bracket = me->get_tax_bracket( iv_annual = lv_annual ).
    lv_tax     = me->calculate_income_tax( iv_annual = lv_annual ).
    lv_net     = lv_annual - lv_tax.

    out->write( |Monthly salary:    { lv_monthly } EUR| ).
    out->write( |Annual gross:      { lv_annual } EUR| ).
    out->write( |Tax bracket:       { lv_bracket }| ).
    out->write( |Estimated tax:     { lv_tax } EUR| ).
    out->write( |Estimated net:     { lv_net } EUR| ).
  ENDMETHOD.

  METHOD calculate_annual_gross.
    rv_annual = iv_monthly * 12.
  ENDMETHOD.

  METHOD get_tax_bracket.
    IF iv_annual <= 10908.
      rv_bracket = 'Tax-free (0%)'.
    ELSEIF iv_annual <= 62809.
      rv_bracket = 'Standard (24%)'.
    ELSEIF iv_annual <= 277825.
      rv_bracket = 'Higher (42%)'.
    ELSE.
      rv_bracket = 'Top (45%)'.
    ENDIF.
  ENDMETHOD.

  METHOD calculate_income_tax.
    " Simplified flat-rate — for learning purposes only
    IF iv_annual <= 10908.
      rv_tax = 0.
    ELSEIF iv_annual <= 62809.
      rv_tax = iv_annual * '0.24'.
    ELSEIF iv_annual <= 277825.
      rv_tax = iv_annual * '0.42'.
    ELSE.
      rv_tax = iv_annual * '0.45'.
    ENDIF.
  ENDMETHOD.
ENDCLASS.
```

### Questions

1. Remove `me->` from `me->calculate_annual_gross(...)` so it reads just `calculate_annual_gross(...)`. Does the code still compile and produce the same output? What does this tell you about when `me->` is required?
2. `get_tax_bracket` and `calculate_income_tax` each contain an `IF / ELSEIF` chain on the same annual-salary thresholds. This is duplication — a bug in one does not automatically fix the other. How could `calculate_income_tax` call `get_tax_bracket` to determine the rate, so the thresholds only exist in one place?
3. Change `lv_monthly` to `'900.00'`. What bracket and tax do you expect? Verify.
4. Add a line to `main` that calculates and displays the monthly net salary (annual net divided by 12). Should you add this in `main` directly or create a new method for it? Justify your answer.

---

## Exercise 4: Two Classes — Product and Order Processor ✋

This exercise has two parts. You will create **two separate ABAP classes** in Eclipse ADT: a helper class `Z_PRODUCT` with no F9 entry point, and a runnable class `Z_ORDER_PROCESSOR` that creates and uses `Z_PRODUCT` objects.

> **Order matters:** Create and activate `Z_PRODUCT` first. `Z_ORDER_PROCESSOR` cannot compile until `Z_PRODUCT` exists.

---

### Part A — Create class `Z_PRODUCT`

This class is a reusable component. It does **not** implement `if_oo_adt_classrun` and cannot be run directly with F9. It represents a single product with a name, price, and category.

```abap
CLASS z_product DEFINITION PUBLIC FINAL CREATE PUBLIC.
  PUBLIC SECTION.
    METHODS constructor
      IMPORTING
        iv_name     TYPE string
        iv_price    TYPE p LENGTH 8 DECIMALS 2
        iv_category TYPE string.
    METHODS get_name
      RETURNING VALUE(rv_name) TYPE string.
    METHODS get_price
      RETURNING VALUE(rv_price) TYPE p LENGTH 8 DECIMALS 2.
    METHODS get_category
      RETURNING VALUE(rv_category) TYPE string.
    METHODS apply_discount
      IMPORTING
        iv_percent TYPE p LENGTH 5 DECIMALS 2.
    METHODS to_display_line
      RETURNING VALUE(rv_line) TYPE string.
  PRIVATE SECTION.
    DATA mv_name     TYPE string.
    DATA mv_price    TYPE p LENGTH 8 DECIMALS 2.
    DATA mv_category TYPE string.
ENDCLASS.

CLASS z_product IMPLEMENTATION.
  METHOD constructor.
    mv_name     = iv_name.
    mv_price    = iv_price.
    mv_category = iv_category.
  ENDMETHOD.

  METHOD get_name.
    rv_name = mv_name.
  ENDMETHOD.

  METHOD get_price.
    rv_price = mv_price.
  ENDMETHOD.

  METHOD get_category.
    rv_category = mv_category.
  ENDMETHOD.

  METHOD apply_discount.
    mv_price = mv_price * ( 1 - iv_percent / 100 ).
  ENDMETHOD.

  METHOD to_display_line.
    rv_line = |{ mv_name } ({ mv_category })  { mv_price } EUR|.
  ENDMETHOD.
ENDCLASS.
```

Activate `Z_PRODUCT` with Ctrl+F3 before proceeding.

---

### Part B — Create class `Z_ORDER_PROCESSOR`

This is the runnable class. It creates `Z_PRODUCT` objects and calculates an order total.

```abap
CLASS z_order_processor DEFINITION PUBLIC FINAL CREATE PUBLIC.
  PUBLIC SECTION.
    INTERFACES if_oo_adt_classrun.
ENDCLASS.

CLASS z_order_processor IMPLEMENTATION.
  METHOD if_oo_adt_classrun~main.
    DATA lo_laptop  TYPE REF TO z_product.
    DATA lo_mouse   TYPE REF TO z_product.
    DATA lo_monitor TYPE REF TO z_product.
    DATA lv_name    TYPE string.
    DATA lv_price   TYPE p LENGTH 8  DECIMALS 2.
    DATA lv_total   TYPE p LENGTH 12 DECIMALS 2.
    DATA lv_qty1    TYPE i VALUE 2.
    DATA lv_qty2    TYPE i VALUE 5.
    DATA lv_qty3    TYPE i VALUE 1.

    " Create three product objects
    lo_laptop  = NEW z_product( iv_name = 'Business Laptop'
                                iv_price = '1299.00'
                                iv_category = 'Computing' ).
    lo_mouse   = NEW z_product( iv_name = 'Wireless Mouse'
                                iv_price = '49.00'
                                iv_category = 'Peripherals' ).
    lo_monitor = NEW z_product( iv_name = '27" Monitor'
                                iv_price = '399.00'
                                iv_category = 'Computing' ).

    " Apply 15% discount to the monitor
    lo_monitor->apply_discount( iv_percent = '15.00' ).

    " Display order lines
    out->write( '--- Order Lines ---' ).
    out->write( |{ lv_qty1 } × { lo_laptop->to_display_line( ) }| ).
    out->write( |{ lv_qty2 } × { lo_mouse->to_display_line( ) }| ).
    out->write( |{ lv_qty3 } × { lo_monitor->to_display_line( ) } (15% discount applied)| ).

    " Calculate order total
    lv_total =   lo_laptop->get_price( )  * lv_qty1
               + lo_mouse->get_price( )   * lv_qty2
               + lo_monitor->get_price( ) * lv_qty3.

    out->write( '' ).
    out->write( |Order total: { lv_total } EUR| ).
  ENDMETHOD.
ENDCLASS.
```

Activate `Z_ORDER_PROCESSOR` with Ctrl+F3, then execute with F9.

### Questions

1. `Z_PRODUCT` does not implement `if_oo_adt_classrun`. What happens if you select it in the Project Explorer and press F9?
2. `lo_laptop` and `lo_monitor` are both declared as `TYPE REF TO z_product`. After `lo_monitor->apply_discount( 15.00 )`, does `lo_laptop`'s price change? Why not?
3. `mv_price` is declared in the PRIVATE section. What would happen if you tried to read `lo_laptop->mv_price` directly from `Z_ORDER_PROCESSOR`? Try it — what does the syntax check say?
4. The `apply_discount` method permanently reduces the stored price. How would you change the design if you needed to keep the original price and apply discounts temporarily? What attribute would you add?

---

## Part E: Independent Challenge — Bank Account

Work through this independently before reading the hints.

### Create class `Z_BANK_ACCOUNT`

Design a bank account class that tracks a balance and processes deposits and withdrawals.

### Required attributes (all PRIVATE)

| Attribute | Type | Description |
|---|---|---|
| `mv_account_no` | `TYPE n LENGTH 10` | Account number |
| `mv_owner` | `TYPE string` | Account holder name |
| `mv_balance` | `TYPE p LENGTH 12 DECIMALS 2` | Current balance |

### Required methods (all PUBLIC)

| Method | Parameters | Description |
|---|---|---|
| `constructor` | `iv_account_no`, `iv_owner`, `iv_initial_deposit` | Initialises the account |
| `deposit` | `IMPORTING iv_amount TYPE p LENGTH 10 DECIMALS 2` | Adds to the balance |
| `withdraw` | `IMPORTING iv_amount TYPE p LENGTH 10 DECIMALS 2` `RETURNING VALUE(rv_success) TYPE abap_bool` | Subtracts if funds are available; returns `abap_true` or `abap_false` |
| `get_balance` | `RETURNING VALUE(rv_balance) TYPE p LENGTH 12 DECIMALS 2` | Returns current balance |
| `get_account_summary` | `RETURNING VALUE(rv_line) TYPE string` | Returns a one-line summary string |

### The runnable `main` method should:

1. Create a `Z_BANK_ACCOUNT` object with an initial deposit of `1000.00 EUR`.
2. Deposit `500.00 EUR`.
3. Withdraw `200.00 EUR` — should succeed.
4. Attempt to withdraw `2000.00 EUR` — should fail (insufficient funds).
5. Display the account summary and final balance.

### Expected output shape

```
Account: 0000000001   Owner: Maria Schneider
Deposit 500.00 EUR        → Balance: 1500.00 EUR
Withdrawal 200.00 EUR     → Balance: 1300.00 EUR
Withdrawal 2000.00 EUR    → FAILED — insufficient funds
Final balance: 1300.00 EUR
```

### Hints

- `abap_bool` is a standard ABAP type. Use `abap_true` for success and `abap_false` for failure.
- Check the withdrawal result: `IF rv_success = abap_true. ... ENDIF.`
- In the constructor, call `me->deposit( iv_amount = iv_initial_deposit ).` to set the opening balance — reuse the method rather than assigning `mv_balance` directly.
- For `get_account_summary`, use a string template combining `mv_account_no` and `mv_owner`.

### Bonus Questions

1. The `withdraw` method returns `abap_bool`. Could you design it to return a string message instead (e.g. `'OK'` or `'INSUFFICIENT FUNDS'`)? What are the trade-offs?
2. Add a transaction log: declare `mt_transactions TYPE TABLE OF string WITH EMPTY KEY` as a private member table. Inside `deposit` and `withdraw`, append a line to `mt_transactions`. Add a method `print_transactions` that loops over the table and writes each line. This previews the pattern from Lecture 4 where data is stored persistently in a database table.

---

## Common Mistakes

### Missing ENDIF or ENDCASE

```abap
IF lv_score >= 90.
  lv_grade = 'A'.
ELSEIF lv_score >= 75.
  lv_grade = 'B'.
" ← ENDIF missing — syntax error
```

Every `IF` needs `ENDIF`. Every `CASE` needs `ENDCASE`. ADT shows exactly which line is missing the closing keyword.

---

### RETURNING Without VALUE( )

```abap
" Wrong — rv_result is a reference parameter, not a copy
CLASS-METHODS double
  RETURNING rv_result TYPE i.

" Correct — VALUE(...) makes it a return copy
CLASS-METHODS double
  RETURNING VALUE(rv_result) TYPE i.
```

Always write `RETURNING VALUE(rv_result)`. Omitting `VALUE(...)` changes the semantics and is a frequent beginner error.

---

### Calling an Instance Method as Static

```abap
" Wrong — get_tax_bracket is an instance method; cannot call without an object
DATA lv_bracket TYPE string.
lv_bracket = z_salary_calculator=>get_tax_bracket( iv_annual = '54000.00' ).

" Correct — use me-> from inside the class, or instantiate first
lv_bracket = me->get_tax_bracket( iv_annual = '54000.00' ).
```

Static methods use `=>`. Instance methods use `->` on a reference, or `me->` from within the same class.

---

### Forgetting to Activate Z_PRODUCT Before Z_ORDER_PROCESSOR

When one class depends on another, activate the dependency first. If `Z_PRODUCT` does not exist in an active state, `Z_ORDER_PROCESSOR` will fail activation with a "type unknown" error on any line that references `z_product`.

---

### Accessing PRIVATE Attributes from Outside

```abap
" Wrong — mv_price is in the PRIVATE section of z_product
DATA lv_price TYPE p LENGTH 8 DECIMALS 2.
lv_price = lo_product->mv_price.   " Syntax error: mv_price not accessible

" Correct — use the PUBLIC getter method
lv_price = lo_product->get_price( ).
```

PRIVATE attributes can only be read or written from within the class that owns them.

---

## Session Summary

| Key Point | What to Remember |
|---|---|
| **IF / CASE** | `IF` for ranges; `CASE` for exact values; never forget `ENDIF` / `ENDCASE` |
| **DO / WHILE** | `DO n TIMES` — fixed count; `WHILE condition` — conditional; guard against infinite loops |
| **sy-index** | `DO` loop counter — 1, 2, 3, … |
| **One method, one purpose** | Keep methods short and focused — readability, reuse, testability |
| **CLASS-METHODS** | Static — called as `class=>method( )` or by name within the same class |
| **METHODS** | Instance — called as `lo_obj->method( )` or `me->method( )` |
| **RETURNING VALUE(...)** | Single return value — always include `VALUE(rv_...)` |
| **PRIVATE section** | Hides data; callers must use PUBLIC methods to access it |
| **constructor** | Runs at `NEW`; sets initial state |
| **NEW** | Creates an object and calls the constructor |
| **Activation order** | Activate dependencies before the classes that use them |

---

## Preparation for Lecture 4

Make sure you can independently write a class that:

1. Defines at least one static method with `IMPORTING` and `RETURNING` parameters.
2. Defines at least two instance methods.
3. Has PRIVATE instance data populated by a constructor.
4. Creates an object with `NEW` and calls its methods.

In Lecture 4, we move from runtime data to the **ABAP Data Dictionary** — the central repository where database tables, data elements, and domains are defined. The data structures you built in Lectures 2 and 3 as local `TYPES` will become persistent database tables, and the queries you write will read real data from the SAP database.

---

## Suggested References

- Horst Keller, *ABAP Objects*, 4th Edition, SAP PRESS, 2020 — Part II: Object-Oriented Programming in depth.
- James Wood, *Object-Oriented Programming with ABAP Objects*, 2nd Edition, SAP PRESS, 2015.
- ABAP Keyword Documentation: [help.sap.com — ABAP Language Reference](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm)
