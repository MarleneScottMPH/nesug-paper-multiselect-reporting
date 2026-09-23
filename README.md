# How to Create a Report for Multi-Select Variables in 4 Steps

## Overview & Citation
* **Paper Title:** How to Create a Report for Multi-Select Variables in 4 Steps
* **Conference:** Northeast SAS Users Group (NESUG) Conference (Burlington, VT)
* **Author:** Marlene Scott, MPH (Quintiles Outcome)
* **Full Text Paper:** Available under [`/docs/How_to_create_a_report_for_multi_select_variables_in_4_steps.pdf`](./docs/How_to_create_a_report_for_multi_select_variables_in_4_steps.pdf)

---

## Executive Summary
In clinical research and epidemiological studies, summarizing binary indicator flags across multiple time windows often presents repetitive coding challenges. This paper demonstrates an efficient, macro-driven solution to summarize frequency of use for **22 drugs across 4 discrete time periods relative to diagnosis (88 total flag variables)** using a fixed denominator ($N = 44$) without invoking individual procedure steps 88 times.

By combining the **SAS DATA Step**, **SAS Macro Language**, **`PROC SQL` Dictionary Tables**, and **ODS RTF control features**, this method automates data structuring, frequency generation, dataset merging, and publication-ready table formatting.

---

## The 4-Step Analytical Framework

1. **Alphabetical Data Structuring via `ATTRIB`:**
   * Split drug flags into 4 temporary datasets corresponding to time periods (`COL1_VARS` through `COL4_VARS`).
   * Used the `ATTRIB` statement within the `DATA` Step to standardize variable attributes and establish alphabetical ordering
     across drug names.

2. **Metadata Extraction & Dynamic Macro Initialization (`PROC SQL`):**
   * Queried `DICTIONARY.COLUMNS` via `PROC SQL` to dynamically populate 88 macro variables (`&COL1VAR1` – `&COL4VAR22`) containing exact drug flag names.
   * Extracted drug base names (`&DRUGNAME1` – `&DRUGNAME22`) using the `SCAN` function within `PROC SQL`.

3. **Macro Iteration & Indirect Macro Variable Referencing (`PROC FREQ`):**
   * Encapsulated `PROC FREQ` inside a macro (`%CATEG_VARS`) and executed it through nested `%DO` loops (`%PUT_IT_TOGETHER`).
   * Employed indirect macro variable references (`&&&&COL&i.VAR&&j.`) to evaluate flag names dynamically and generate count/percentage
     datasets for all 88 flags.
   * Merged time-period datasets by `BLOCK` and `LINE` using a second macro (`%MERGEFILES`) with indirect references (`F_&&&DRUGNAME&k..1YPREV`, etc.) to yield 22 unified drug datasets.

4. **ODS RTF Formatting & Output Generation (`PROC REPORT`):**
   * Applied `ODS ESCAPECHAR="^"` to format text dynamically (e.g., `^S={font_weight=bold} Drug A^S={}^nBrand Name A`) for bolding
     and inline page breaks.
   * Defined a custom ODS RTF style template (`styles.outcome`) modifying margins, stripping table/header background colors, removing
     gridlines, and overriding font attributes to match target table shell specifications.

---

## Technical Features & SAS Syntax
* **SAS Base & Data Step:** `ATTRIB` statement, `DATA` step dataset concatenation/merging.
* **Macro Processor:** `%MACRO`, nested `%DO` loops, double ampersand (`&&`) and quadruple ampersand (`&&&&`) indirect macro variable resolution.
* **Metadata Processing:** `PROC SQL`, `DICTIONARY.COLUMNS`, `SCAN` function text isolation.
* **Output Delivery System (ODS):** `ODS ESCAPECHAR` inline formatting, PROC TEMPLATE RTF style modification, `PROC REPORT`.
