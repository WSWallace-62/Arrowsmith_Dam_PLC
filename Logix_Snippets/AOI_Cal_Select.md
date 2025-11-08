# Valve Calibration AOI - Selector Logic

## Overview
Three conditional rungs that enable only one valve calibration AOI instance at a time based on the `CAL_Valve_Select` tag value.

---

## Rung 0: Bypass Valve Calibration

**Rung Comment:**
```
Bypass Valve Calibration - Only active when CAL_Valve_Select = 1. Uses shared CAL_ control/status tags with Bypass-specific InOut parameters.
```

**L5X Rung String:**
```
EQU(CAL_Valve_Select,1)Valve_Position_Calibration(CAL_Enable,CAL_Confirm_CMD,CAL_Abort_CMD,ZSC_10_302,ZSO_10_302,60000,500,10000,MOV_10_302_OP,MOV_10_302_CL,CAL_In_Progress,CAL_Complete,CAL_Aborted,CAL_Failed,CAL_Awaiting_Confirm,CAL_Step,Bypass_Position_Counter,Bypass_InRawMin,Bypass_InRawMax,Bypass_Full_Stroke_Time_SP,Bypass_Position_Percent);
```

---

## Rung 1: Main Valve Calibration

**Rung Comment:**
```
Main Valve Calibration - Only active when CAL_Valve_Select = 2. Uses shared CAL_ control/status tags with Main-specific InOut parameters.
```

**L5X Rung String:**
```
EQU(CAL_Valve_Select,2)Valve_Position_Calibration(CAL_Enable,CAL_Confirm_CMD,CAL_Abort_CMD,ZSC_10_300,ZSO_10_300,60000,500,10000,MOV_10_300_OP,MOV_10_300_CL,CAL_In_Progress,CAL_Complete,CAL_Aborted,CAL_Failed,CAL_Awaiting_Confirm,CAL_Step,Main_Position_Counter,Main_InRawMin,Main_InRawMax,Main_Full_Stroke_Time_SP,Main_Position_Percent);
```

---

## Rung 2: Siphon Valve Calibration

**Rung Comment:**
```
Siphon Valve Calibration - Only active when CAL_Valve_Select = 3. Uses shared CAL_ control/status tags with Siphon-specific InOut parameters.
```

**L5X Rung String:**
```
EQU(CAL_Valve_Select,3)Valve_Position_Calibration(CAL_Enable,CAL_Confirm_CMD,CAL_Abort_CMD,ZSC_10_301,ZSO_10_301,60000,500,10000,MOV_10_301_OP,MOV_10_301_CL,CAL_In_Progress,CAL_Complete,CAL_Aborted,CAL_Failed,CAL_Awaiting_Confirm,CAL_Step,Siphon_Position_Counter,Siphon_InRawMin,Siphon_InRawMax,Siphon_Full_Stroke_Time_SP,Siphon_Position_Percent);
```

---

## Required Tag

**Tag Name:** `CAL_Valve_Select`  
**Data Type:** DINT  
**Initial Value:** 0  
**Tag Comment:**
```
Valve selector for calibration: 0=None, 1=Bypass, 2=Main, 3=Siphon
```

---

## Implementation Notes

- Place these three rungs in the `Valve_Pos_Cal` routine
- Only ONE AOI instance will be active at any time
- When `CAL_Valve_Select = 0`, all AOI instances are disabled
- All three valves share the same `CAL_` control and status tags
- Each valve uses its own unique InOut parameter tags (Position_Counter, InRawMin, InRawMax, etc.)
- HMI provides dropdown/selector to set `CAL_Valve_Select` value

---

## Valve-Specific I/O Mapping

| Valve   | Closed LS  | Open LS    | Open Output  | Close Output |
|---------|------------|------------|--------------|--------------|
| Bypass  | ZSC_10_302 | ZSO_10_302 | MOV_10_302_OP| MOV_10_302_CL|
| Main    | ZSC_10_300 | ZSO_10_300 | MOV_10_300_OP| MOV_10_300_CL|
| Siphon  | ZSC_10_301 | ZSO_10_301 | MOV_10_301_OP| MOV_10_301_CL|

---

**Date Created:** November 8, 2025  
**Related Documentation:** Valve_Calibration_AOI_Implementation_Summary.md
