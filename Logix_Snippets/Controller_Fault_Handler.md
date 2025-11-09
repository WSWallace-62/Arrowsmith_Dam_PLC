# Controller Fault Handler - Auto-Clear Power-Up Faults

## Purpose
Automatically clear Type 01 Code 01 (Power lost and restored in RUN mode) faults to allow the controller to resume operation without manual intervention after a power cycle.

## Implementation

### Rung 0 - Auto-Clear Power-Up Fault

**Rung Comment:**
```
Check if most recent fault is Type 01 (Power-up Fault) Code 01 (Power lost and restored in RUN). If true, clear the fault automatically to allow controller to resume RUN mode without operator intervention.
```

**L5X Rung String:**
```
EQU(FAULTLOG[0].Type,1)EQU(FAULTLOG[0].Code,1)CLF();
```

**Alternative (more explicit fault clearing):**
```
EQU(FAULTLOG[0].Type,1)EQU(FAULTLOG[0].Code,1)GSV(FaultLog,0,MajorFaultBits,Fault_MajorBits)CLR(Fault_MajorBits);
```

## Tag Definitions

If using the alternative method with GSV instruction:

**Tag Name:** `Fault_MajorBits`
**Data Type:** `DINT`
**Tag Comment:**
```
Temporary storage for major fault bits during fault handler execution
```

## How It Works

1. **Fault occurs** - Controller detects power loss/restore while in RUN mode
2. **Fault Handler executes** - Before controller goes to faulted state
3. **Logic checks** - Verifies fault is Type 01, Code 01 (power-up fault)
4. **CLF() instruction** - Clears the fault automatically
5. **Controller resumes** - Returns to RUN mode without manual intervention

## Fault Log Structure

**FAULTLOG[0]** contains the most recent fault:
- `FAULTLOG[0].Type` - Fault type (1 = Power-up Fault)
- `FAULTLOG[0].Code` - Fault code (1 = Power lost and restored in RUN mode)
- `FAULTLOG[0].Info` - Additional fault information

## Notes

- This only auto-clears **Type 01 Code 01** faults (benign power interruptions)
- All other major faults still require manual clearing (safety feature)
- Controller Fault Handler executes **before** controller enters faulted state
- Power-Up Handler still executes normally before fault evaluation
- Fault is still logged in fault history for diagnostic purposes

## Safety Considerations

This is appropriate for remote/unmanned sites where:
- Brief power interruptions are expected
- Automatic recovery is desired
- Operators are notified of power events through SCADA alarms
- Other critical faults still halt operation and require intervention

For manned facilities or safety-critical applications, consider leaving manual fault clearing enabled to ensure operator awareness of all fault conditions.
