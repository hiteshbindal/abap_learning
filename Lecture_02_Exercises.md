# Lecture 2 — Exercises: Data Types, Structures, and Internal Tables

**Module:** M-SAP-WP-1.2 Client-Server Communication with SAP ABAP  
**Programme:** SAP Engineering and Analytics (M.Sc.)

> **Before you start:** Read `Lecture_02_Theory.md` first. These exercises assume you understand the ABAP type system, structures, internal tables, field symbols, and type conversion traps.

---

## Learning Objectives

After these exercises, you should be able to:

1. Declare variables with the correct elementary type for a given piece of business data.
2. Define a structure type and create a data object from it.
3. Build an internal table, append rows, and loop over them.
4. Use a field symbol loop to modify rows in place.
5. Recognise and explain integer truncation and string-to-number conversion errors.

---

## Reminder: The BTP Class Template

All programs in this course use the class pattern. Copy this skeleton for every new class:

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

Output with string templates: `out->write( |text { variable }| ).`  
Blank line in output: `out->write( '' ).`

---

## Exercise 1: Elementary Types — Declare and Display ✋

### Goal

Declare one variable for each elementary type, assign a meaningful business value, and display all variables with labels.

### Create class `Z_DATA_TYPES_OVERVIEW` in your package.

```abap
CLASS z_data_types_overview DEFINITION PUBLIC FINAL CREATE PUBLIC.
  PUBLIC SECTION.
    INTERFACES if_oo_adt_classrun.
ENDCLASS.

CLASS z_data_types_overview IMPLEMENTATION.
  METHOD if_oo_adt_classrun~main.
    DATA lv_country     TYPE c LENGTH 3.
    DATA lv_emp_id      TYPE n LENGTH 8.
    DATA lv_quantity    TYPE i.
    DATA lv_price       TYPE p LENGTH 8 DECIMALS 2.
    DATA lv_date        TYPE d.
    DATA lv_time        TYPE t.
    DATA lv_description TYPE string.

    lv_country     = 'DEU'.
    lv_emp_id      = 1001.
    lv_quantity    = 50.
    lv_price       = '299.99'.
    lv_date        = '20250101'.
    lv_time        = '090000'.
    lv_description = 'Ergonomic office chair'.

    out->write( |Country:     { lv_country }| ).
    out->write( |Employee ID: { lv_emp_id }| ).
    out->write( |Quantity:    { lv_quantity }| ).
    out->write( |Price:       { lv_price }| ).
    out->write( |Date:        { lv_date }| ).
    out->write( |Time:        { lv_time }| ).
    out->write( |Description: { lv_description }| ).
  ENDMETHOD.
ENDCLASS.
```

Activate (Ctrl+F3) and execute (F9).

### Questions

1. What output do you see for `lv_emp_id = 1001`? Why does it look like `00001001`?
2. Change `lv_date` to `sy-datum` instead of a fixed value. What happens?
3. What happens if you try `lv_emp_id = 'ABCD'`? Run the syntax check (Ctrl+F2) and read the error.

---

## Exercise 2: Employee Structure ✋

### Goal

Define a structure type for an employee and display its fields using string templates.

### Create class `Z_EMPLOYEE_STRUCTURE`.

```abap
CLASS z_employee_structure DEFINITION PUBLIC FINAL CREATE PUBLIC.
  PUBLIC SECTION.
    INTERFACES if_oo_adt_classrun.
ENDCLASS.

CLASS z_employee_structure IMPLEMENTATION.
  METHOD if_oo_adt_classrun~main.
    TYPES: BEGIN OF ty_employee,
      employee_id TYPE n LENGTH 8,
      first_name  TYPE string,
      last_name   TYPE string,
      department  TYPE c LENGTH 4,
      salary      TYPE p LENGTH 8 DECIMALS 2,
      hire_date   TYPE d,
    END OF ty_employee.

    DATA ls_employee TYPE ty_employee.

    ls_employee-employee_id = '00000042'.
    ls_employee-first_name  = 'Maria'.
    ls_employee-last_name   = 'Schneider'.
    ls_employee-department  = 'SALE'.
    ls_employee-salary      = '3500.00'.
    ls_employee-hire_date   = '20230901'.

    out->write( |Employee ID: { ls_employee-employee_id }| ).
    out->write( |Name:        { ls_employee-first_name } { ls_employee-last_name }| ).
    out->write( |Department:  { ls_employee-department }| ).
    out->write( |Salary:      { ls_employee-salary } EUR| ).
    out->write( |Hired:       { ls_employee-hire_date }| ).
  ENDMETHOD.
ENDCLASS.
```

### Questions

1. What is the difference between `ty_employee` and `ls_employee` in the code above?
2. Change `ls_employee-salary` to `'not a number'`. What does the syntax check say?
3. Add a second employee variable `ls_manager TYPE ty_employee` and display their details too. What does this show about reusing type definitions?

---

## Exercise 3: Internal Table — Build and Display ✋

### Goal

Build an internal table of employees, display all rows, then display only a filtered subset.

### Create class `Z_EMPLOYEE_TABLE`.

```abap
CLASS z_employee_table DEFINITION PUBLIC FINAL CREATE PUBLIC.
  PUBLIC SECTION.
    INTERFACES if_oo_adt_classrun.
ENDCLASS.

CLASS z_employee_table IMPLEMENTATION.
  METHOD if_oo_adt_classrun~main.
    TYPES: BEGIN OF ty_employee,
      employee_id TYPE n LENGTH 8,
      first_name  TYPE string,
      last_name   TYPE string,
      department  TYPE c LENGTH 4,
      salary      TYPE p LENGTH 8 DECIMALS 2,
    END OF ty_employee.

    DATA ls_employee  TYPE ty_employee.
    DATA lt_employees TYPE TABLE OF ty_employee WITH EMPTY KEY.

    " Add employees
    ls_employee-employee_id = '00000001'.
    ls_employee-first_name  = 'Maria'.
    ls_employee-last_name   = 'Schneider'.
    ls_employee-department  = 'SALE'.
    ls_employee-salary      = '3500.00'.
    APPEND ls_employee TO lt_employees.

    ls_employee-employee_id = '00000002'.
    ls_employee-first_name  = 'Thomas'.
    ls_employee-last_name   = 'Bauer'.
    ls_employee-department  = 'FIN'.
    ls_employee-salary      = '4200.00'.
    APPEND ls_employee TO lt_employees.

    ls_employee-employee_id = '00000003'.
    ls_employee-first_name  = 'Aiko'.
    ls_employee-last_name   = 'Tanaka'.
    ls_employee-department  = 'SALE'.
    ls_employee-salary      = '3800.00'.
    APPEND ls_employee TO lt_employees.

    " Display all employees
    out->write( '--- All Employees ---' ).
    LOOP AT lt_employees INTO ls_employee.
      out->write( |{ ls_employee-employee_id }  { ls_employee-first_name } { ls_employee-last_name }  { ls_employee-department }  { ls_employee-salary } EUR| ).
    ENDLOOP.

    out->write( '' ).

    " Display Sales department only
    out->write( '--- Sales Department ---' ).
    LOOP AT lt_employees INTO ls_employee WHERE department = 'SALE'.
      out->write( |{ ls_employee-first_name } { ls_employee-last_name }| ).
    ENDLOOP.
  ENDMETHOD.
ENDCLASS.
```

### Questions

1. How many rows does `lt_employees` contain after the three `APPEND` statements?
2. Which employees appear in the second `LOOP`? Why?
3. Add `SORT lt_employees BY last_name.` before the first LOOP. How does the output change?
4. What happens if you `APPEND ls_employee TO lt_employees` without changing any fields first?

---

## Exercise 4: Field Symbol — Modify Rows In Place ✋

### Goal

Extend the employee table from Exercise 3: give every Sales employee a 5% salary increase using a field symbol, then display the updated table.

### Create class `Z_EMPLOYEE_SALARY_UPDATE`.

```abap
CLASS z_employee_salary_update DEFINITION PUBLIC FINAL CREATE PUBLIC.
  PUBLIC SECTION.
    INTERFACES if_oo_adt_classrun.
ENDCLASS.

CLASS z_employee_salary_update IMPLEMENTATION.
  METHOD if_oo_adt_classrun~main.
    TYPES: BEGIN OF ty_employee,
      employee_id TYPE n LENGTH 8,
      first_name  TYPE string,
      last_name   TYPE string,
      department  TYPE c LENGTH 4,
      salary      TYPE p LENGTH 8 DECIMALS 2,
    END OF ty_employee.

    DATA ls_employee  TYPE ty_employee.
    DATA lt_employees TYPE TABLE OF ty_employee WITH EMPTY KEY.
    FIELD-SYMBOLS: <fs_employee> TYPE ty_employee.

    " Build the table
    ls_employee-employee_id = '00000001'.
    ls_employee-first_name  = 'Maria'.
    ls_employee-last_name   = 'Schneider'.
    ls_employee-department  = 'SALE'.
    ls_employee-salary      = '3500.00'.
    APPEND ls_employee TO lt_employees.

    ls_employee-employee_id = '00000002'.
    ls_employee-first_name  = 'Thomas'.
    ls_employee-last_name   = 'Bauer'.
    ls_employee-department  = 'FIN'.
    ls_employee-salary      = '4200.00'.
    APPEND ls_employee TO lt_employees.

    ls_employee-employee_id = '00000003'.
    ls_employee-first_name  = 'Aiko'.
    ls_employee-last_name   = 'Tanaka'.
    ls_employee-department  = 'SALE'.
    ls_employee-salary      = '3800.00'.
    APPEND ls_employee TO lt_employees.

    " Apply 5% increase to Sales department
    LOOP AT lt_employees ASSIGNING <fs_employee>
      WHERE department = 'SALE'.
      <fs_employee>-salary = <fs_employee>-salary * '1.05'.
    ENDLOOP.

    " Display updated table
    out->write( '--- Updated Salaries ---' ).
    LOOP AT lt_employees INTO ls_employee.
      out->write( |{ ls_employee-first_name } { ls_employee-last_name }  ({ ls_employee-department })  { ls_employee-salary } EUR| ).
    ENDLOOP.
  ENDMETHOD.
ENDCLASS.
```

### Questions

1. Thomas Bauer is in the FIN department. Did his salary change? Why not?
2. What would happen if you used `INTO ls_employee` instead of `ASSIGNING <fs_employee>` in the salary update loop — and left out the MODIFY statement?
3. Replace `ASSIGNING <fs_employee>` with `INTO ls_employee` and add the MODIFY statement. Does the result change?

---

## Exercise 5: Type Conversion ✋

### Goal

Demonstrate integer truncation and correct type usage for decimal results.

### Create class `Z_TYPE_CONVERSION`.

```abap
CLASS z_type_conversion DEFINITION PUBLIC FINAL CREATE PUBLIC.
  PUBLIC SECTION.
    INTERFACES if_oo_adt_classrun.
ENDCLASS.

CLASS z_type_conversion IMPLEMENTATION.
  METHOD if_oo_adt_classrun~main.
    DATA lv_int         TYPE i     VALUE 2025.
    DATA lv_text        TYPE string.
    DATA lv_amount      TYPE p LENGTH 8 DECIMALS 2.
    DATA lv_input       TYPE string VALUE '1499.90'.
    DATA lv_date        TYPE d VALUE '20250401'.
    DATA lv_year        TYPE n LENGTH 4.
    DATA lv_month       TYPE n LENGTH 2.
    DATA lv_result_int  TYPE i.
    DATA lv_result_dec  TYPE p LENGTH 8 DECIMALS 2.

    " 1. Integer to string — automatic conversion
    lv_text = lv_int.
    out->write( |Integer as string: { lv_text }| ).

    " 2. String to packed decimal
    lv_amount = lv_input.
    out->write( |Packed amount: { lv_amount }| ).

    " 3. Date parts via offset/length access
    lv_year  = lv_date(4).
    lv_month = lv_date+4(2).
    out->write( |Date: { lv_date }  Year: { lv_year }  Month: { lv_month }| ).

    " 4. Integer truncation
    lv_result_int = 10 / 3.
    out->write( |10 / 3 as TYPE i: { lv_result_int }| ).

    " 5. Same division with decimal result
    lv_result_dec = 10 / 3.
    out->write( |10 / 3 as TYPE p: { lv_result_dec }| ).
  ENDMETHOD.
ENDCLASS.
```

### Questions

1. What is the value of `lv_result_int`? Why?
2. What is the value of `lv_result_dec`? What does this tell you about type choice for averages?
3. What do you expect to happen if you change `lv_input` to `'not a number'`? Try it — what error appears in the console?

---

## Part F: Independent Challenge — Payroll Calculation

Work through this independently before looking for hints.

### Create class `Z_PAYROLL_CALC`

### Define a structure with fields for:

- Employee ID (`n LENGTH 8`)
- Full name (`string`)
- Department (`c LENGTH 4`)
- Monthly base salary (`p LENGTH 8 DECIMALS 2`)
- Bonus percentage (`p LENGTH 5 DECIMALS 2`)

### Build an internal table with at least four employees from at least two departments.

### Calculate and display for each employee:

- Annual gross salary (monthly × 12)
- Bonus amount (salary × bonus percentage / 100)

### Then calculate and display:

- Total annual payroll cost across all employees
- The highest salary in the table

### Expected output shape:

```
--- Payroll Summary ---
00000001  Maria Schneider     SALE  Annual: 42000.00 EUR  Bonus: 2100.00 EUR
00000002  Thomas Bauer        FIN   Annual: 50400.00 EUR  Bonus: 2520.00 EUR
...

Total payroll:   xxx.xx EUR
Highest salary:  xxx.xx EUR/month
```

### Hints

- Declare `lv_total` and `lv_highest` as `TYPE p LENGTH 12 DECIMALS 2`.
- To find the highest salary, loop through the table and use an `IF` to check whether each salary is greater than the current highest.
- `SORT lt_employees BY salary DESCENDING.` followed by `READ TABLE lt_employees INTO ls_employee INDEX 1.` also gives you the highest — try both approaches.

### Bonus Question

What is the difference between these two loops? Which is correct here, and why?

```abap
" Loop A
LOOP AT lt_employees INTO ls_employee.
  ls_employee-salary = ls_employee-salary * 12.
ENDLOOP.

" Loop B
LOOP AT lt_employees ASSIGNING <fs_employee>.
  <fs_employee>-salary = <fs_employee>-salary * 12.
ENDLOOP.
```

---

## Common Mistakes

### Using `TYPE i` for Monetary Amounts

```abap
DATA lv_price TYPE i.
lv_price = '19.99'.   " Stored as 19 — decimal part lost silently
```

Always use `TYPE p LENGTH 8 DECIMALS 2` for prices and financial amounts.

---

### Using `TYPE c` Where `TYPE n` Is Correct

```abap
DATA lv_order_id TYPE c LENGTH 10.
lv_order_id = '42'.   " Stored as '42        ' — trailing spaces
```

Document numbers and IDs that consist entirely of digits should use `n`:

```abap
DATA lv_order_id TYPE n LENGTH 10.
lv_order_id = 42.   " Stored as '0000000042' — leading zeros, correct
```

---

### Forgetting APPEND

```abap
ls_employee-first_name = 'Maria'.
ls_employee-salary     = '3500.00'.
" APPEND ls_employee TO lt_employees.   ← forgotten!

LOOP AT lt_employees INTO ls_employee.
  out->write( ls_employee-first_name ).   " Nothing displays — table is empty
ENDLOOP.
```

Add `out->write( |Rows in table: { lines( lt_employees ) }| ).` after building the table to confirm rows were added.

---

### Modifying with INTO but Missing MODIFY

```abap
" This does NOT update the table — the change is lost after ENDLOOP
LOOP AT lt_employees INTO ls_employee.
  ls_employee-salary = ls_employee-salary * '1.05'.
  " Missing: MODIFY lt_employees FROM ls_employee.
ENDLOOP.
```

Either add `MODIFY lt_employees FROM ls_employee.` inside the loop, or switch to `ASSIGNING <fs_employee>`.

---

## Session Summary

| Key Point | What to Remember |
|---|---|
| **`p` for money** | Never use `i` for prices or amounts — decimals are truncated silently |
| **`n` for IDs** | Use `n` for document numbers that need leading zeros |
| **`d` for dates** | `YYYYMMDD` format; `sy-datum` gives today; arithmetic works directly |
| **Structure** | Blueprint defined with `TYPES: BEGIN OF ... END OF`; variables use `ls_` prefix |
| **Internal table** | `APPEND` adds rows; `LOOP AT ... INTO` reads; `LOOP AT ... ASSIGNING` modifies |
| **Field symbol** | Direct reference into the table; changes apply immediately without MODIFY |
| **Integer truncation** | `7 / 2 = 3` — use `p` for any result that may have decimals |

---

## Preparation for Lecture 3

Make sure you can independently write a class that:

1. Defines a structure with at least four fields of different types.
2. Creates an internal table of that type.
3. Appends at least three rows.
4. Loops over all rows and displays the fields.
5. Uses a field symbol loop to modify one numeric field in every row.

In Lecture 3, we add control structures (`IF`, `CASE`, `DO`, `WHILE`) and start writing real program logic that branches and repeats.

---

## Suggested References

- Horst Keller, *ABAP Objects*, 4th Edition, SAP PRESS, 2020 — Chapters 4–5: data types and internal tables in depth.
- Paul Hardy, *ABAP to the Future*, 3rd Edition, SAP PRESS, 2019 — Chapter 3: modern ABAP data declarations.
- ABAP Keyword Documentation: [help.sap.com — ABAP Language Reference](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm)
