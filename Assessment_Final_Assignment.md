# Assessment: Final Project Report

**Module:** M-SAP-WP-1.2 Client-Server Communication with SAP ABAP  
**Programme:** SAP Engineering and Analytics (M.Sc.)  
**Language:** English  
**Audience:** All enrolled students

---

## 1. Overview

This module is assessed entirely through a **Project Report**. There is no written examination.

The report demonstrates that you can connect the lecture content to a focused topic of your choosing, engage meaningfully with primary sources, and present technical material in clear, structured written English.

> **Key point:** The report is not a summary of lecture slides. It is an independent piece of technical writing that shows you can analyse, compare, and critically evaluate a topic within the SAP ABAP domain.

---

## 2. Project Report Requirements

### 2.1 Formal Requirements

| Item | Requirement |
|---|---|
| Length | 10–15 pages (body text only — see below) |
| Language | English |
| Format | Microsoft Word (.docx) or PDF |
| Submission | Via the learning management system (Moodle / equivalent) |
| Deadline | As communicated by the module coordinator |

**Pages excluded from the 10–15 page count:**

- Cover page
- Table of contents
- Reference list / bibliography
- Appendices (optional — use sparingly)

### 2.2 Required Report Structure

Your report must follow this structure. Suggested page allocations are given as guidance, not hard limits.

1. **Introduction** *(~1 page)*  
   State the motivation for your topic choice, define the scope, and formulate a clear research question or thesis statement. Example: *"This report examines whether ABAP Objects offers measurable maintainability advantages over classical ABAP in large-scale SAP projects."*

2. **Theoretical Background** *(3–4 pages)*  
   Introduce the relevant SAP and ABAP concepts that underpin your topic. This section demonstrates your understanding of the technical foundation. Reference lecture content, but go further by drawing on books and SAP documentation.

3. **Technical Analysis / Implementation** *(4–6 pages)*  
   This is the core of your report. Depending on your topic, this section may include:
   - Code comparisons (before/after, two paradigms, two approaches)
   - Architecture or design diagrams
   - A step-by-step implementation walkthrough
   - Benchmark or trace analysis results
   - A case study or hypothetical project scenario

   Include **at least three meaningful ABAP code examples**, each introduced and explained in the text.

4. **Critical Evaluation** *(2–3 pages)*  
   Analyse the strengths, weaknesses, limitations, and trade-offs of your chosen topic or approach. Offer best-practice recommendations grounded in your research. Avoid unsubstantiated claims.

5. **Conclusion** *(~1 page)*  
   Summarise your findings in relation to your research question. Identify open questions or areas for future investigation.

6. **References**  
   Minimum **five sources**. Acceptable source types: SAP official documentation (help.sap.com), SAP PRESS books, SAP Community blogs (community.sap.com), peer-reviewed papers. Wikipedia is **not** an acceptable primary source. All sources cited in the body text must appear here; all listed references must be cited in the text.

---

## 3. Topic Options

Choose **one** topic from the list below. You may propose an alternative topic to the module coordinator for approval, provided it falls within the scope of SAP ABAP development.

---

### Topic 1: Evolution of ABAP

ABAP has transformed over four decades from a simple procedural batch language into a modern, object-oriented, cloud-ready platform. This topic traces that evolution — from classical ABAP in the 1980s, through the introduction of ABAP Objects in 1999, to the modern constructs introduced in ABAP 7.4 and 7.5 (inline declarations, constructor operators, string templates), and onwards to ABAP for SAP HANA, CDS Views, and the RESTful ABAP Programming Model (RAP). Understanding this evolution helps developers make informed decisions about which techniques to apply in new and existing projects.

**Primary lectures:** 1, 2, 3  
**Secondary lectures:** 4, 7

**Suggested angles:**
- Compare a classical ABAP solution (FORM routines, TYPE c fields) with its modern equivalent (classes, inline DATA, string processing)
- Analyse the migration challenges when moving legacy code to modern ABAP
- Evaluate the impact of SAP HANA's column-store architecture on ABAP programming rules

---

### Topic 2: ABAP Objects vs Classical ABAP

Classical ABAP relies on FORM routines, function modules, and global data. ABAP Objects introduced encapsulation, inheritance, interfaces, and polymorphism. This topic makes a rigorous technical comparison of the two paradigms: what problems does OO ABAP solve, where does it introduce complexity, and what is the real-world adoption rate in SAP projects? The report should include practical code examples demonstrating the same business logic implemented in both paradigms, and evaluate the difference in testability (ABAP Unit), maintainability, and reuse.

**Primary lectures:** 3  
**Secondary lectures:** 1, 2, 6

**Suggested angles:**
- Side-by-side implementation: a customer validation routine as FORM vs. as a class method
- Testability: how ABAP Unit testing requires OO design (dependency injection, interfaces)
- Survey of real SAP codebases — what proportion of custom code uses OO patterns?

---

### Topic 3: Performance Optimization Techniques

SAP systems process millions of records; a single poorly written SELECT can degrade system performance for all users. This topic provides a deep technical analysis of ABAP performance: SQL optimization (WHERE clauses, field selection, bulk fetch, JOIN vs. FOR ALL ENTRIES), internal table type selection (STANDARD, SORTED, HASHED, secondary keys), application-server buffering, and the use of runtime analysis tools (ST05 SQL Trace, SAT ABAP Runtime Analysis). The report should include before-and-after code comparisons with quantified or estimated performance impacts.

**Primary lectures:** 7  
**Secondary lectures:** 1, 4

**Suggested angles:**
- Benchmark: measure execution time of nested SELECT vs. FOR ALL ENTRIES for increasing dataset sizes
- HANA push-down: how CDS views and AMDP move computation to the database layer
- Cost model: estimate roundtrip costs for different access patterns

---

### Topic 4: SAP Enhancements Framework

When SAP delivers standard software, customers cannot safely modify the delivered source code — every support package risks overwriting such changes. SAP's Enhancement Framework provides upgrade-safe extension points: User Exits, Customer Exits, Enhancement Spots, BAdIs (classic and kernel). This topic analyses each technique in depth, compares their capabilities and limitations, and presents a case study of implementing a BAdI in a real or simulated purchasing or sales scenario. The report should make a clear argument for why BAdIs are the recommended approach for most scenarios.

**Primary lectures:** 6  
**Secondary lectures:** 3, 4

**Suggested angles:**
- Comparison table: technique, upgrade safety, API stability, testability, typical use case
- Step-by-step BAdI implementation with test strategy
- Analyse what happens to each technique type during an S/4HANA upgrade

---

### Topic 5: ABAP and SAP S/4HANA

SAP S/4HANA is the successor to SAP ECC, built on the SAP HANA in-memory database. The transition has significant implications for ABAP developers: some classical coding patterns are deprecated (SELECT * on large tables, SELECT...ENDSELECT, direct access to certain compatibility views), new data models are simpler (Universal Journal in FI), and new opportunities arise (ABAP CDS, AMDP, Embedded Analytics). This topic analyses what concretely changes for an ABAP developer moving from ECC to S/4HANA, using the SAP Custom Code Migration tools and ABAP Test Cockpit findings as evidence.

**Primary lectures:** 1, 4, 7  
**Secondary lectures:** 5, 6

**Suggested angles:**
- Catalogue deprecated statements and their S/4HANA replacements
- Analyse the simplified data model (e.g. removal of BSEG/BKPF separation) and its ABAP impact
- Evaluate the SAP Custom Code Migration app as a migration tooling approach

---

### Topic 6: CDS Views and Modern ABAP

Core Data Services (CDS) is a framework for defining semantically rich data models directly in the ABAP development environment. CDS views replace classical database views (SE11), support OData exposure for Fiori applications, provide built-in access control (DCL), and enable push-down calculations to SAP HANA. This topic introduces CDS syntax, compares CDS views to classical views on key dimensions (performance, reusability, OData readiness, maintenance), and demonstrates a worked example of a CDS view exposed as a Fiori List Report.

**Primary lectures:** 1, 4, 7  
**Secondary lectures:** 5

**Suggested angles:**
- Syntax walkthrough: a classical JOIN view re-implemented as a CDS view
- Annotations: @OData.publish, @UI, @Semantics — how they drive Fiori UI generation
- Access control: CDS DCL vs. classical authorization checks in ABAP

---

### Topic 7: RAP — RESTful ABAP Programming Model

RAP is SAP's strategic programming model for building transactional Fiori applications on SAP BTP and S/4HANA. It combines CDS-based data modelling, behavior definitions (BDEF), behavior implementations (ABAP classes), and OData V4 service bindings into a unified development approach. This topic introduces the RAP concept, walks through building a simple "business object" (e.g. a course enrollment object), and contrasts the RAP approach with classical Dynpro programming in terms of architecture, effort, and maintainability.

**Primary lectures:** 3, 4, 5  
**Secondary lectures:** 1, 6

**Suggested angles:**
- Contrast: classical Dynpro module pool vs. RAP-based Fiori app for the same use case
- RAP "Hello World": step-by-step from CDS entity to running Fiori tile
- Evaluate RAP's managed vs. unmanaged scenario — when to use which

---

## 4. Evaluation Criteria

Reports are graded on a German university scale of 1.0 (excellent) to 5.0 (fail). A grade of 4.0 or better is required to pass the module.

| Criterion | Weight | Description |
|---|---|---|
| **Structure and clarity** | 15% | Logical flow between sections; clear section headings; academic writing style; appropriate use of figures and tables; no significant language errors |
| **Technical accuracy** | 30% | All ABAP concepts and statements are correct; code examples are syntactically valid and would compile; no factual errors about SAP products or processes |
| **Depth of analysis** | 25% | Goes significantly beyond lecture material; engages critically with SAP documentation and published books; presents a clear argument or thesis rather than merely describing |
| **Code examples** | 15% | At least three meaningful, working ABAP code snippets; each is properly introduced, explained, and connected to the surrounding argument |
| **References** | 15% | Minimum five acceptable sources; all cited in-text using a consistent citation style (APA or IEEE recommended); no Wikipedia as primary source |

> **Note on technical accuracy:** Even a well-written report will receive a reduced grade if the ABAP code examples contain syntax errors or if SAP concepts are described incorrectly. Test your code examples in ADT or the SAP system before submitting.

---

## 5. Topic–Lecture Coverage Matrix

Use this matrix to identify which lectures are most relevant for your chosen topic. **P** = primary relevance, **S** = secondary relevance.

| Topic | L1 | L2 | L3 | L4 | L5 | L6 | L7 |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 1 — Evolution of ABAP | P | P | P | S | — | — | S |
| 2 — ABAP Objects vs Classical | S | S | P | — | — | S | — |
| 3 — Performance Optimization | P | — | — | S | — | — | P |
| 4 — Enhancements Framework | — | — | S | S | — | P | — |
| 5 — ABAP and S/4HANA | P | — | — | P | S | S | P |
| 6 — CDS Views and Modern ABAP | P | — | — | P | S | — | P |
| 7 — RAP | S | — | P | P | P | S | — |

---

## 6. Suggested Timeline

The module runs over approximately eight weeks alongside the lecture programme. The following timeline is a recommendation, not a hard schedule.

| Week | Activity |
|---|---|
| **1–2** | Attend Lectures 1–2; choose your topic; begin reading the mandatory textbook chapters relevant to your topic |
| **3–4** | Attend Lectures 3–4; conduct primary research: SAP documentation, SAP PRESS books, SAP Community blogs; take notes with source references |
| **5–6** | Attend Lectures 5–6; draft Report Sections 2 and 3 (theoretical background and technical analysis); write and test all code examples |
| **7** | Attend Lecture 7; draft Sections 1, 4, and 5 (introduction, critical evaluation, conclusion); compile reference list |
| **8** | Review complete draft; proofread for language and technical accuracy; format consistently; submit before deadline |

> **Recommendation:** Do not leave writing to the final week. A well-structured argument takes time to develop. Starting your outline in Week 1 — even if only a few bullet points — will significantly improve the final result.

---

## 7. Academic Integrity

All submitted work must comply with the academic integrity policy of Hochschule Fresenius.

- All text must be your own original writing or properly cited quotation.
- All code examples must be original. You may reference and adapt examples from SAP documentation or textbooks, but you must cite the source and explain the adaptation.
- **Use of AI-generated text** (e.g. from large language models) must be declared explicitly in a short statement on the cover page, with a description of how it was used. Undeclared AI-generated content is treated as plagiarism.
- Plagiarism — submitting the work of another person as your own, or reproducing source text without citation — results in a grade of 5.0 (fail) for the module and may have further academic consequences.

---

## 8. Recommended Resources

The following resources support all seven topic options. Obtain the mandatory textbook before beginning your research.

### Mandatory

- Horst Keller, *ABAP Objects: Introduction to Programming SAP Applications*, 4th Edition, SAP PRESS, 2020.  
  Comprehensive reference for all ABAP language concepts. Relevant chapters vary by topic.

### Further Reading

- Paul Hardy, *ABAP to the Future*, 3rd Edition, SAP PRESS, 2019.  
  Covers modern ABAP syntax, CDS, RAP, and SAP HANA-specific development. Particularly relevant for Topics 1, 5, 6, 7.

- Thomas Schneider, *Getting Started with ABAP: Beginner's Guide*, SAP PRESS, 2016.  
  Accessible introduction to all core ABAP topics. Useful supplementary reference for Topics 1–4.

- James Wood, *Object-Oriented Programming with ABAP Objects*, 2nd Edition, SAP PRESS, 2015.  
  In-depth OO ABAP reference. Essential for Topic 2.

- Bernd Matzke, *SAP ABAP: Hands-on Test Projects with Business Scenarios*, Galileo Press, 2016.  
  Worked examples connecting ABAP programming to realistic business cases.

### Online Resources

- **SAP Help Portal** — [help.sap.com](https://help.sap.com): official ABAP language reference, keyword documentation, CDS and RAP guides.
- **SAP Community** — [community.sap.com](https://community.sap.com): practitioner blog posts, Q&A, and technical articles. Cite specific posts with author, title, and date.
- **SAP Learning Hub** — if available through your institution, provides structured learning paths and official SAP courseware.

> All URLs accessed from online resources must include the date of access in your reference list.

---

## Session Summary

The assessment for this module rewards independent research and technical depth. The seven topic options each connect directly to the lecture programme, so the best preparation for the report is active engagement with the lectures and the mandatory textbook throughout the semester.

Choose a topic that genuinely interests you — the quality of analysis in a report written with curiosity is consistently higher than one written purely to satisfy requirements.

**Good luck.**
