# Revision Notes - Documentation Requirements

This document outlines the standards and requirements for creating revision notes for the Arrowsmith Dam PLC project.

---

## General Requirements

### File Naming Convention
- **Format:** `Revisions_[DD-MMM-YYYY].md`
- **Example:** `Revisions_12-Nov-2025.md`
- **Location:** `/Revision_Notes/` folder

### Header Format
```markdown
# Arrowsmith Dam PLC - Revision Notes
**Date:** [Full date format: Month DD, YYYY]
**Revised By:** [Full name]
```

---

## Content Requirements

### 1. Focus on Actual PLC Changes Only
- Document only changes made to the PLC program files (L5X files)
- **Do NOT include:**
  - Documentation updates (IO checksheets, spreadsheets)
  - Summary sections
  - "Next steps" or recommendations
  - Issues identified but not fixed

### 2. Change Categories

Document changes under appropriate headings:

#### I/O Point Comment Corrections
- **What:** Changes to I/O point descriptions/comments in the program
- **Include:**
  - I/O address (Slot, Channel)
  - OLD comment text
  - NEW comment text
  - Reason for change

**Example:**
```markdown
**Local:2:I.PT12.DATA** (Slot 2, Channel 12):
- **OLD:** "Siphon Valve Fully Closed"
- **NEW:** "Siphon Valve Fully Opened"
- **Reason:** Comment was incorrect; this input is mapped to `ZSO_10_301` (fully opened limit switch)
```

#### Tag Renaming
- **What:** Changes to tag names for clarity or consistency
- **Include:**
  - Old tag name → New tag name
  - Location (Routine name and Rung number - NOT L5X line numbers)
  - Tag mapping/logic
  - Reason for change
  - Associated I/O point

**Example:**
```markdown
**HMI_Main → PowerOn_HMI:**
- **Location:** Tag declaration and Y_Outputs_To_HMI routine (Rung 55)
- **Mapping:** `XIC(JSH_10_308)OTE(PowerOn_HMI);`
- **Reason:** Original name "HMI_Main" was ambiguous and conflicted with Main Valve tag naming convention. New name clearly indicates this is AC Power ON status to HMI.
- **I/O:** Slot 1, Channel 15 (AC Power ON relay contact)
```

#### Logic Changes
- **What:** Modifications to ladder logic, function blocks, or program flow
- **Include:**
  - Routine name and Rung number
  - Description of the change
  - Before/After logic (if applicable)
  - Reason for change

#### New Tags or Functions
- **What:** Addition of new tags, routines, or AOIs
- **Include:**
  - Tag name and data type
  - Purpose/function
  - Associated routine or location

---

## Formatting Standards

### Reference Conventions

**DO:**
- Use routine names and rung numbers: "Y_Outputs_To_HMI routine (Rung 55)"
- Use I/O addresses: "Slot 2, Channel 12"
- Use tag names in backticks: `` `PowerOn_HMI` ``
- Use clear before/after formatting: "OLD: ... NEW: ..."

**DO NOT:**
- Reference L5X file line numbers
- Include vague descriptions
- Use abbreviations without defining them first

### Markdown Formatting

- Use **bold** for field names (OLD:, NEW:, Location:, Reason:)
- Use backticks for code/tag names: `` `ZSO_10_301` ``
- Use appropriate heading levels (## for main sections, ### for subsections, #### for specific changes)
- Use bullet points for lists of changes
- Use code blocks for logic snippets

---

## Quality Checklist

Before finalizing revision notes, verify:

- [ ] Date and author are correct
- [ ] Only PLC program changes are documented
- [ ] All changes include clear reason/justification
- [ ] Routine and rung numbers are referenced (not L5X line numbers)
- [ ] Tag names use backticks for clarity
- [ ] Before/after states are clearly documented
- [ ] No documentation-only changes are included
- [ ] Formatting is consistent throughout
- [ ] No summary or "next steps" sections

---

## Example Structure

```markdown
# Arrowsmith Dam PLC - Revision Notes
**Date:** November 12, 2025
**Revised By:** Scott Wallace

---

## PLC Program Changes

### I/O Point Comment Corrections

**Local:2:I.PT12.DATA** (Slot 2, Channel 12):
- **OLD:** "Siphon Valve Fully Closed"
- **NEW:** "Siphon Valve Fully Opened"
- **Reason:** Comment was incorrect; this input is mapped to `ZSO_10_301`

### Tag Renaming for Clarity

**HMI_Main → PowerOn_HMI:**
- **Location:** Y_Outputs_To_HMI routine (Rung 55)
- **Mapping:** `XIC(JSH_10_308)OTE(PowerOn_HMI);`
- **Reason:** Original name was ambiguous
- **I/O:** Slot 1, Channel 15 (AC Power ON)
```

---

## Notes

- Keep revision notes concise but complete
- Focus on **what** changed and **why** it changed
- Assume the reader has access to the PLC program files for detailed logic review
- Revision notes are for tracking changes, not for troubleshooting or future planning
