# Assessment: Written Examination (Klausur)

**Module:** M-SAP-WP-1.2 Client-Server Communication with SAP ABAP  
**Programme:** SAP Engineering and Analytics (M.Sc.)  
**Language:** English  
**Audience:** All enrolled students

---

## 1. Overview

This module is assessed entirely through a **Written Examination (Klausur)**. There is no project report or academic paper.

The exam tests your ability to recall, apply, and reason about the ABAP concepts covered across all seven lectures. Questions are designed to demonstrate understanding, not rote memorisation — you will be expected to read, analyse, and write short ABAP code, explain design decisions, and compare techniques.

> **Key point:** The exam covers all lecture content. Active engagement with the lectures, the mandatory textbook, and the hands-on exercises throughout the semester is the most effective preparation.

---

## 2. Exam Format

| Item | Details |
|---|---|
| Duration | 90 minutes |
| Permitted aids | None (closed-book) |
| Language | English |
| Location | As communicated by the module coordinator |
| Date | As communicated by the module coordinator |

**No aids are permitted during the examination.** This includes notes, printed materials, textbooks, electronic devices, and calculators. Formula sheets or syntax reference cards will not be provided. Students are expected to have working knowledge of ABAP syntax and SAP concepts from memory.

---

## 3. Exam Structure and Question Types

The exam consists of **three sections** totalling **100 marks**.

---

### Section A — Knowledge and Comprehension *(30 marks)*

Short-answer questions testing factual recall and understanding of concepts from all lectures.

**Format:** 10 questions × 3 marks each  
**Expected response length:** 2–5 sentences per question

**Example questions:**
- What is the difference between a transparent table and a pooled table in the ABAP Data Dictionary?
- Name three internal table types available in ABAP and state one use case where each is appropriate.
- What is a BAdI and how does it differ from a classic User Exit?
- Explain the purpose of the PBO and PAI events in Dynpro programming.
- What does the `FOR ALL ENTRIES` addition to `SELECT` do, and what is one important precaution when using it?

---

### Section B — Code Reading and Analysis *(40 marks)*

Two code-analysis tasks. Each presents an ABAP code snippet and asks you to read, explain, or critique it.

**Format:** 2 tasks × 20 marks each  
**Expected response length:** Structured written answer with specific references to the code

**What is tested:**
- Correct identification of what a code snippet does and why
- Spotting errors, inefficiencies, or deprecated patterns
- Suggesting improvements with justification
- Tracing the flow of data through ABAP structures (internal tables, field symbols, class methods)

**Example task:**
> The following ABAP program reads all open sales orders into an internal table inside a loop. Identify the performance problem, explain why it causes an issue, and rewrite the relevant lines to correct it. (20 marks)

---

### Section C — Applied Concepts *(30 marks)*

One extended question requiring you to design or explain an ABAP solution for a described business scenario.

**Format:** 1 question, multiple sub-parts  
**Expected response length:** Structured written answer; ABAP pseudocode or real syntax may be used

**What is tested:**
- Selecting the appropriate ABAP technique for a given requirement
- Justifying design decisions with reference to lecture concepts
- Understanding architectural implications (modularisation, enhancements, UI choice)

**Example question:**
> A company needs to add a custom validation step to a standard SAP procurement transaction without modifying SAP standard code. (a) Which enhancement technique would you recommend and why? (b) Sketch the implementation steps. (c) What risks does this approach carry compared with a direct modification? (30 marks)

---

## 4. Topics and Lecture Coverage

All seven lectures are examinable. The weight of each topic in the exam broadly reflects the number of lectures dedicated to it.

| Lecture | Topic | Approximate exam weight |
|---|---|:---:|
| 1 | Introduction to SAP and ABAP | ~10% |
| 2 | Data Types and Data Objects | ~15% |
| 3 | Modularization Techniques | ~15% |
| 4 | ABAP Data Dictionary | ~15% |
| 5 | User Interface Programming | ~15% |
| 6 | Enhancements and Modifications | ~15% |
| 7 | Performance Tuning and Best Practices | ~15% |

> Questions frequently combine concepts from multiple lectures. For example, a performance question may require knowledge of both internal table types (Lecture 2) and SELECT optimisation (Lecture 7).

---

## 5. Evaluation and Grading

The exam is graded on a German university scale of 1.0 (excellent) to 5.0 (fail). A grade of 4.0 or better is required to pass the module.

| Grade | Marks required | Description |
|---|---|---|
| 1.0 | 95–100 | Exceptional — thorough, precise, well-reasoned answers |
| 1.3 | 90–94 | |
| 1.7 | 85–89 | |
| 2.0 | 80–84 | Good — solid understanding with minor gaps |
| 2.3 | 75–79 | |
| 2.7 | 70–74 | |
| 3.0 | 65–69 | Satisfactory — adequate coverage with some inaccuracies |
| 3.3 | 60–64 | |
| 3.7 | 55–59 | |
| 4.0 | 50–54 | Sufficient — minimum pass threshold |
| 5.0 | < 50 | Fail |

**Marking guidance:**

- **Section A:** Full marks for responses that are factually correct and complete. Partial credit is awarded for partially correct answers.
- **Section B:** Marks are awarded for correct identification of problems, accuracy of explanation, and quality of the proposed fix. A correct fix with no explanation earns half marks at most.
- **Section C:** Marks are distributed across sub-parts as indicated in the exam paper. Justifications must reference specific ABAP or SAP concepts to receive full marks.

> **Note on code accuracy:** In Sections B and C, ABAP code you write is assessed on correctness of logic and syntax. Minor syntactic slips (e.g. a missing period) may be overlooked if the intent is clear, but logical errors will reduce your mark. Write only what you know — an incorrect fix is worse than leaving a blank.

---

## 6. Preparation Guidance

### 6.1 Suggested Study Approach

| Week | Activity |
|---|---|
| **1–2** | Review lecture notes for Lectures 1–2; re-attempt any in-lecture exercises |
| **3–4** | Review Lectures 3–4; write out key syntax from memory (FORM routines, function module calls, class definitions); practice reading unfamiliar code snippets |
| **5–6** | Review Lectures 5–6; practice explaining UI techniques and enhancement types in your own words |
| **7** | Review Lecture 7; practise identifying performance anti-patterns and their corrections |
| **8** | Full revision across all topics; work through past exercises without notes; time yourself |

### 6.2 Effective Preparation Strategies

- **Write code by hand.** During the exam you will have no IDE, no autocomplete, and no syntax check. Practice writing ABAP statements on paper or without running them in ADT. Focus on SELECT, internal table operations, class definitions, and CALL FUNCTION / CALL METHOD.
- **Know the "why" not just the "what".** Questions ask for justification. Know why `FOR ALL ENTRIES` outperforms nested SELECTs, why BAdIs are preferred over direct modifications, why HASHED tables suit certain access patterns.
- **Use the lecture structure as a checklist.** Each lecture has a defined set of concepts. Work through each systematically and ask yourself: Can I explain this concept? Can I write an example? Can I identify a mistake involving it?
- **Focus on comparison questions.** The exam frequently asks you to compare two techniques (e.g. FORM vs. method, classical view vs. CDS, STANDARD vs. SORTED table). Prepare comparison notes for the major contrasts covered in lectures.

---

## 7. Recommended Resources

The following resources support revision across all topics.

### Mandatory

- Horst Keller, *ABAP Objects: Introduction to Programming SAP Applications*, 4th Edition, SAP PRESS, 2020.  
  The primary reference for all ABAP language concepts covered in the lectures. Relevant chapters should be reviewed in parallel with each lecture.

### Further Reading

- Paul Hardy, *ABAP to the Future*, 3rd Edition, SAP PRESS, 2019.  
  Covers modern ABAP syntax, CDS, RAP, and SAP HANA-specific development. Particularly useful for Lectures 1, 5, 6, 7.

- Thomas Schneider, *Getting Started with ABAP: Beginner's Guide*, SAP PRESS, 2016.  
  Accessible overview of core ABAP topics. Useful for consolidating foundational concepts from Lectures 1–4.

- James Wood, *Object-Oriented Programming with ABAP Objects*, 2nd Edition, SAP PRESS, 2015.  
  In-depth OO ABAP reference. Essential for Lecture 3 (Modularization).

### Online Resources

- **SAP Help Portal** — [help.sap.com](https://help.sap.com): official ABAP keyword documentation. Use as a reference during study; it will not be available during the exam.
- **SAP Community** — [community.sap.com](https://community.sap.com): practitioner blog posts and technical Q&A. Useful for understanding real-world application of exam topics.

---

## 8. Academic Integrity

All examination work must comply with the academic integrity policy of Hochschule Fresenius.

- The exam must be completed entirely independently. No collaboration with other students is permitted during or after the examination.
- Use of any electronic device, notes, or printed material during the examination constitutes an exam violation and results in a grade of 5.0 (fail) for the module, which may have further academic consequences.
- Impersonation — having another person take the exam on your behalf — is treated as examination fraud.

---

## Session Summary

The written examination rewards thorough, active engagement with the lecture material throughout the semester. It is not possible to prepare adequately by studying only in the final week.

Work through the exercises in each lecture, practice writing ABAP code from memory, and focus on understanding the reasoning behind each technique — not just its syntax.

**Good luck.**
