# Calibration Alarm Outputs to HMI

**Routine:** Y_Outputs_To_HMI  
**Date Created:** November 10, 2025  
**Purpose:** Transfer individual valve calibration required warning statuses from X_Alarms routine to HMI interface tags

---

## Overview

These rungs copy the six individual valve calibration required warning bits to dedicated HMI tags. These warnings are generated in the X_Alarms routine (rungs 18-23) when valve position calibration is needed due to high or low position tracking drift.

The general calibration alarm (`Cal_Required_General_Alarm`) is already sent to HMI on rung 16. These additional rungs provide individual valve-specific warning details for operator awareness.

---

## Logic Implementation

### Rung 17 - Bypass High Calibration Warning

**L5X Rung String:**
```
XIC(Bypass_HighCal_Required_Warning)OTE(Bypass_HighCal_Required_Warning_HMI);
```

**Rung Comment:**
```
Bypass Valve high calibration required warning status to HMI
```

**New Tag: BOOL**

Tag Name:
```
Bypass_HighCal_Required_Warning_HMI
```

Tag Comment:
```
Bypass Valve high calibration required warning status for HMI display
```

---

### Rung 18 - Bypass Low Calibration Warning

**L5X Rung String:**
```
XIC(Bypass_LowCal_Required_Warning)OTE(Bypass_LowCal_Required_Warning_HMI);
```

**Rung Comment:**
```
Bypass Valve low calibration required warning status to HMI
```

**New Tag: BOOL**

Tag Name:
```
Bypass_LowCal_Required_Warning_HMI
```

Tag Comment:
```
Bypass Valve low calibration required warning status for HMI display
```

---

### Rung 19 - Main High Calibration Warning

**L5X Rung String:**
```
XIC(Main_HighCal_Required_Warning)OTE(Main_HighCal_Required_Warning_HMI);
```

**Rung Comment:**
```
Main Valve high calibration required warning status to HMI
```

**New Tag: BOOL**

Tag Name:
```
Main_HighCal_Required_Warning_HMI
```

Tag Comment:
```
Main Valve high calibration required warning status for HMI display
```

---

### Rung 20 - Main Low Calibration Warning

**L5X Rung String:**
```
XIC(Main_LowCal_Required_Warning)OTE(Main_LowCal_Required_Warning_HMI);
```

**Rung Comment:**
```
Main Valve low calibration required warning status to HMI
```

**New Tag: BOOL**

Tag Name:
```
Main_LowCal_Required_Warning_HMI
```

Tag Comment:
```
Main Valve low calibration required warning status for HMI display
```

---

### Rung 21 - Siphon High Calibration Warning

**L5X Rung String:**
```
XIC(Siphon_HighCal_Required_Warning)OTE(Siphon_HighCal_Required_Warning_HMI);
```

**Rung Comment:**
```
Siphon Valve high calibration required warning status to HMI
```

**New Tag: BOOL**

Tag Name:
```
Siphon_HighCal_Required_Warning_HMI
```

Tag Comment:
```
Siphon Valve high calibration required warning status for HMI display
```

---

### Rung 22 - Siphon Low Calibration Warning

**L5X Rung String:**
```
XIC(Siphon_LowCal_Required_Warning)OTE(Siphon_LowCal_Required_Warning_HMI);
```

**Rung Comment:**
```
Siphon Valve low calibration required warning status to HMI
```

**New Tag: BOOL**

Tag Name:
```
Siphon_LowCal_Required_Warning_HMI
```

Tag Comment:
```
Siphon Valve low calibration required warning status for HMI display
```

---

## Implementation Notes

- Insert these six rungs after rung 16 in the Y_Outputs_To_HMI routine
- Current rung 17 ("Generator Data to HMI" header) will become rung 23
- Source warning bits are set in X_Alarms routine rungs 18-23
- All tags use standard HMI suffix convention (_HMI) for interface tags
- These warnings indicate when valve position calibration should be performed to maintain accurate position tracking

---

## Related Logic

- **Source Routine:** X_Alarms (rungs 18-23) - Individual calibration warnings
- **Source Routine:** X_Alarms (rung 24) - General calibration alarm (OR of all six individual warnings)
- **Related Routine:** Valve_Pos_Cal - Semi-automatic calibration procedure using AOI
