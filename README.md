# ABAP Learning — M-SAP-WP-1.2

**Module:** M-SAP-WP-1.2 Client-Server Communication with SAP ABAP  
**Programme:** SAP Engineering and Analytics (M.Sc.)  
**Institution:** Hochschule Fresenius  

---

## Course Overview

This repository contains all lecture notes, code examples, and illustrated exercises for the ABAP module. Each lecture is designed for a **2-hour session** (approximately 1 hour of guided content + 1 hour of hands-on exercises).

All images are Eclipse ADT / SAP GUI mockups generated to illustrate key concepts without requiring a live SAP system for reading the notes.

> **Official course details** (provided by colleague):
> [M-SAP-WP-1.2 Client-Server Communication with SAP ABAP — Course PDF](docs/M-SAP-WP-1.2_Course_Details.pdf)

---

## Lectures

| # | Title | Key Topics |
|---|---|---|
| [01](Lecture_01_Introduction_to_SAP_and_ABAP.md) | Introduction to SAP and ABAP | ERP overview, AS ABAP architecture, Hello World, variables, arithmetic |
| [02](Lecture_02_Data_Types_and_Data_Objects.md) | Data Types and Data Objects | c, n, i, p, d, t, string, structures, internal tables, field symbols, type conversions |
| [03](Lecture_03_Modularization_Techniques.md) | Modularization Techniques | FORM routines, function modules, includes, macros, OO ABAP classes and methods |
| [04](Lecture_04_ABAP_Data_Dictionary.md) | ABAP Data Dictionary (DDIC) | Database tables, domains, data elements, views, indexes, search helps, lock objects |
| [05](Lecture_05_User_Interface_Programming.md) | User Interface Programming | Selection screens, classical Dynpro PBO/PAI, ALV, messages, Web Dynpro overview |
| [06](Lecture_06_Enhancements_and_Modifications.md) | Enhancements and Modifications | User exits, customer exits, BAdIs, enhancement framework, upgrade-safe extension techniques |
| [07](Lecture_07_Performance_Tuning_and_Best_Practices.md) | Performance Tuning and Best Practices | SQL optimization, nested SELECT anti-patterns, FOR ALL ENTRIES, internal table types, ST05, SAT |

---

## Assessment

**Format:** Written Examination (Klausur)

The examination format for this module has been changed from a project report to a written examination for the current semester, following a decision by the Examination Committee.

Details (duration, permitted aids, and registration) will be communicated by your lecturer and confirmed via the official Hochschule Fresenius examination office.

---

## Repository Structure

```
/
├── Lecture_01_Introduction_to_SAP_and_ABAP.md
├── Lecture_02_Data_Types_and_Data_Objects.md
├── Lecture_03_Modularization_Techniques.md
├── Lecture_04_ABAP_Data_Dictionary.md
├── Lecture_05_User_Interface_Programming.md
├── Lecture_06_Enhancements_and_Modifications.md
├── Lecture_07_Performance_Tuning_and_Best_Practices.md
├── README.md
└── images/
    ├── adt_01_*.png  ...  adt_08_*.png   (Lecture 01)
    ├── lec02_01_*.png ... lec02_08_*.png (Lecture 02)
    ├── lec03_01_*.png ... lec03_08_*.png (Lecture 03)
    ├── lec04_01_*.png ... lec04_08_*.png (Lecture 04)
    ├── lec05_01_*.png ... lec05_08_*.png (Lecture 05)
    ├── lec06_01_*.png ... lec06_08_*.png (Lecture 06)
    └── lec07_01_*.png ... lec07_08_*.png (Lecture 07)
```

---

## Recommended Reading

- Horst Keller, *ABAP Objects*, 4th Edition, SAP PRESS, 2020
- Paul Hardy, *ABAP to the Future*, 3rd Edition, SAP PRESS, 2019
- Thomas Schneider, *Getting Started with ABAP*, SAP PRESS, 2016
- SAP official ABAP documentation: [help.sap.com](https://help.sap.com)
