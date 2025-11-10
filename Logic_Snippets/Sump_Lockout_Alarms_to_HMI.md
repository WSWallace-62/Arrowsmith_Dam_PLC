# Sump Pump Lockout Alarms to HMI

## Description
Sends Meter Chamber and Valve Chamber Sump Pump lockout alarm status to the HMI for display and annunciation.

## Implementation Date
November 10, 2025

## Routine
Y_Outputs_To_HMI

## Location
Added after Rung 34 (Sump Pump data section)

---

## Logic

### Rung 35

**L5X Rung String:**
```
XIC(MeterSump_Lockout_Alarm)OTE(MeterSump_Lockout_Alarm_HMI);
```

**Rung Comment:**
```
Meter Chamber Sump Pump lockout alarm status to HMI
```

**New Tag: BOOL**

Tag Name:
```
MeterSump_Lockout_Alarm_HMI
```

Tag Comment:
```
Meter Chamber Sump lockout alarm for HMI display
```

---

### Rung 36

**L5X Rung String:**
```
XIC(ValveSump_Lockout_Alarm)OTE(ValveSump_Lockout_Alarm_HMI);
```

**Rung Comment:**
```
Valve Chamber Sump Pump lockout alarm status to HMI
```

**New Tag: BOOL**

Tag Name:
```
ValveSump_Lockout_Alarm_HMI
```

Tag Comment:
```
Valve Chamber Sump lockout alarm for HMI display
```

---

## C-more Database Tags

Add these tags to the C-more database (CmoreDB-Nov10R0.CSV):

| Tag Name | Data Type | Address |
|----------|-----------|---------|
| MeterSump_Lockout_Alarm_HMI | Discrete | BOOLMeterSump_Lockout_Alarm_HMI |
| ValveSump_Lockout_Alarm_HMI | Discrete | BOOLValveSump_Lockout_Alarm_HMI |

---

## Source Signals

| Alarm Tag | Source | Description |
|-----------|--------|-------------|
| MeterSump_Lockout_Alarm | X_Alarms Rung 2 | Indicates Meter Chamber Sump exceeded 30-min runtime limit |
| ValveSump_Lockout_Alarm | X_Alarms Rung 1 | Indicates Valve Chamber Sump exceeded 30-min runtime limit |

---

## Notes

- These alarms are set in the X_Alarms routine when sump pumps exceed their 30-minute maximum runtime protection
- The lockout condition prevents pump operation for 60 minutes after the fault
- Existing C-more database has tags without _HMI suffix; database will need to be updated to use these new tag names
- Existing rungs 35-51 will be renumbered to 37-53 after inserting these two new rungs
- Related lockout countdown timers already sent to HMI: `MeterSump_Lockout_Rem_Min_HMI` and `ValveSump_Lockout_Rem_Min_HMI`
