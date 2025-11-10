# Genset Alarms to HMI

## Description
Sends Genset fault and low fuel alarm status to the HMI for display and alarming.

## Implementation Date
November 10, 2025

## Routine
Y_Outputs_To_HMI

## Location
Added after Rung 25 (Genset data section)

---

## Logic

### Rung 26

**L5X Rung String:**
```
XIC(GA_10_303)OTE(Genset_Fault_HMI);
```

**Rung Comment:**
```
Genset fault status to HMI
```

**New Tag:**
```
Genset_Fault_HMI, BOOL
Genset fault indication for HMI display
```

---

### Rung 27

**L5X Rung String:**
```
XIC(LAL_10_303)OTE(Genset_LowFuel_HMI);
```

**Rung Comment:**
```
Genset low fuel alarm status to HMI
```

**New Tag:**
```
Genset_LowFuel_HMI, BOOL
Genset low fuel alarm for HMI display
```

---

## C-more Database Tags

Add these tags to the C-more database (CmoreDB-Nov10R0.CSV):

| Tag Name | Data Type | Address |
|----------|-----------|---------|
| Genset_Fault_HMI | Discrete | BOOLGenset_Fault_HMI |
| Genset_LowFuel_HMI | Discrete | BOOLGenset_LowFuel_HMI |

---

## Source Signals

| Input Tag | I/O Address | Description |
|-----------|-------------|-------------|
| GA_10_303 | Local:1:I.Pt01 | Genset Fault (relay contact) |
| LAL_10_303 | Local:1:I.Pt02 | Genset Low Fuel (alarm contact) |

---

## Notes

- These signals are already mapped from inputs in the A_Input_Mapping routine
- They are used for status display on the HMI
- Future implementation may include alarm logic in the X_Alarms routine for latching and annunciation
- Existing rungs 26-49 will be renumbered to 28-51 after inserting these two new rungs
