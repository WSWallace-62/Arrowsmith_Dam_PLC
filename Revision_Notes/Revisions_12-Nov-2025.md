# Arrowsmith Dam PLC - Revision Notes
**Date:** November 12, 2025  
**Revised By:** Scott Wallace

---

## PLC Program Changes


#### 1. Corrected I/O Comments

**Local:2:I.PT12.DATA** (Slot 2, Channel 12):<br>
Location: Tag declaration and A_Input_mapping routine (Rung 28)

- **OLD:** "Siphon Valve Fully Closed"
- **NEW:** "Siphon Valve Fully Opened"
- **Reason:** Comment was incorrect.

**Local:2:I.PT13.DATA** (Slot 2, Channel 13):<br>
Location: Tag declaration and A_Input_mapping routine (Rung 29)

- **OLD:** (Missing comment)
- **NEW:** "Siphon Valve Fully Closed"
- **Reason:** Added missing comment.

#### 2. Tags Renamed

**HMI_Main → PowerOn_HMI:**
- **Location:** Tag declaration and Y_Outputs_To_HMI routine (Rung 55)
- **Reason:** Original name "HMI_Main" didn't make sense. New name is correct.
- **I/O:** Slot 1, Channel 15 (AC Power ON relay contact)

**VTS_Main_VTS → PowerOn_VTS:**
- **Location:** Tag declaration and Z_Outputs_To_VTS routine (Rung 55)
- **Reason:** Original name "VTS_Main_VTS" didn't make sense. New name is correct.
- **I/O:** Slot 1, Channel 15 (AC Power ON relay contact)

---
