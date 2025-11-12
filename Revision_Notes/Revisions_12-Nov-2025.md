# Arrowsmith Dam PLC - Revision Notes
**Date:** November 12, 2025  
**Revised By:** Scott Wallace

---

## PLC Program Changes


#### 1. Corrected I/O Comments

**Local:2:I.PT12.DATA** (Slot 2, Channel 12):
**Location: Tag declaration and A_Input_mapping routine (Rung 28)
- **OLD:** "Siphon Valve Fully Closed"
- **NEW:** "Siphon Valve Fully Opened"
- **Reason:** Comment was incorrect; this input is mapped to `ZSO_10_301` (fully opened limit switch)

**Local:2:I.PT13.DATA** (Slot 2, Channel 13):
**Location: Tag declaration and A_Input_mapping routine (Rung 29)
- **OLD:** (Missing comment)
- **NEW:** "Siphon Valve Fully Closed"
- **Reason:** Added missing comment; this input is mapped to `ZSC_10_301` (fully closed limit switch)

#### 2. Tag Renaming for Clarity

**HMI_Main → PowerOn_HMI:**
- **Location:** Tag declaration and Y_Outputs_To_HMI routine (Rung 55)
- **Mapping:** `XIC(JSH_10_308)OTE(PowerOn_HMI);`
- **Reason:** Original name "HMI_Main" was ambiguous and conflicted with Main Valve tag naming convention. New name clearly indicates this is AC Power ON status to HMI.
- **I/O:** Slot 1, Channel 15 (AC Power ON relay contact)

**VTS_Main_VTS → PowerOn_VTS:**
- **Location:** Tag declaration and Z_Outputs_To_VTS routine (Rung 55)
- **Mapping:** `XIC(JSH_10_308)OTE(PowerOn_VTS);`
- **Reason:** Matched naming convention with HMI tag for consistency. Clearly indicates AC Power ON status to VTS/SCADA.
- **I/O:** Slot 1, Channel 15 (AC Power ON relay contact)

---
