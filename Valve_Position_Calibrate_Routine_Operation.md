# Valve Position Calibrate Routine Operation

## Overview
The `Valve_Position_Calibrate_Routine_RLL_Nov6_R0` is a state machine-based calibration sequence for the Bypass Valve (MOV-10-302). This routine automatically calibrates the valve's position feedback system by performing a full stroke cycle from closed to open, measuring the stroke time, and calculating the position increment value used for position tracking.

## Purpose
The calibration routine establishes accurate position feedback for the bypass valve by:
1. Establishing a zero reference point (fully closed position)
2. Measuring the full stroke time from closed to open
3. Calculating the position increment value based on the stroke time
4. Storing the calibrated value in `Bypass_Position_Increment`

## State Machine Operation

### STATE 0: IDLE (Default State)
**Rung 0**

**Condition:** `DEV_Cal_Step = 0`

**Operation:**
- The routine waits in this state until the operator initiates calibration
- When `DEV_Calibrate_Enable` is activated (operator presses the 'Calibrate' button), the routine:
  - Sets `DEV_Awaiting_Confirm` flag
  - Starts the `DEV_Confirmation_Timer` (10-second countdown)
  - Advances to STATE 5

**Tags Involved:**
- `DEV_Cal_Step`: Current state number
- `DEV_Calibrate_Enable`: HMI calibration enable button
- `DEV_Awaiting_Confirm`: Flag indicating waiting for confirmation
- `DEV_Confirmation_Timer`: 10-second confirmation window timer
- `DEV_ONS[0]`: One-shot to prevent multiple triggers

---

### STATE 5: AWAITING CONFIRMATION
**Rung 1**

**Condition:** `DEV_Cal_Step = 5`

**Operation:**
The system provides a 10-second window for the operator to confirm the calibration action. This is a safety feature to prevent accidental calibration starts.

**Two Possible Outcomes:**

1. **Operator Confirms** (`DEV_Confirm_CMD` activated):
   - Resets `DEV_Confirmation_Timer`
   - Sets `DEV_Cal_In_Progress` flag
   - Clears `DEV_Awaiting_Confirm` flag
   - Advances to STATE 10 (begin calibration)

2. **Timeout** (`DEV_Confirmation_Timer.DN` true after 10 seconds):
   - Clears `DEV_Awaiting_Confirm` flag
   - Returns to STATE 0 (Idle)
   - Calibration is cancelled

**Tags Involved:**
- `DEV_Confirm_CMD`: HMI confirmation button
- `DEV_Confirmation_Timer.DN`: Timer done bit (10 seconds elapsed)
- `DEV_Cal_In_Progress`: Flag indicating calibration is active
- `DEV_ONS[1]`: One-shot for confirmation

---

### STATE 10: DRIVE TO CLOSE
**Rung 2**

**Condition:** `DEV_Cal_Step = 10`

**Operation:**
The valve is commanded to the fully closed position to establish a zero reference point for position measurement.

**Logic:**

1. **If valve is fully closed** (`ZSC_10_302` true):
   - Resets `DEV_Timeout_Timer`
   - Advances to STATE 20

2. **If valve is not fully closed** (`ZSC_10_302` false):
   - Energizes `MOV_10_302_CL` (close output)
   - Runs `DEV_Timeout_Timer` as a safety timeout

**Tags Involved:**
- `ZSC_10_302`: Bypass valve fully closed limit switch input
- `MOV_10_302_CL`: Output to close the bypass valve
- `DEV_Timeout_Timer`: Safety timeout timer

**Safety Feature:** If the timeout expires before the closed limit is reached, the fault logic (Rung 9) triggers and moves to STATE 99.

---

### STATE 20: RESET AND PREPARE
**Rung 3**

**Condition:** `DEV_Cal_Step = 20`

**Operation:**
Preparation step that resets measurement values before the calibration stroke begins.

**Actions:**
- Resets `DEV_Stroke_Timer` (accumulated time cleared)
- Clears `Bypass_Position_Counter` (sets to 0)
- Immediately advances to STATE 30

**Tags Involved:**
- `DEV_Stroke_Timer`: Timer that will measure the stroke time
- `Bypass_Position_Counter`: Raw position counter value

---

### STATE 30: DRIVE TO OPEN & MEASURE STROKE
**Rung 4**

**Condition:** `DEV_Cal_Step = 30`

**Operation:**
The valve is commanded to fully open while the stroke time is measured. This is the critical measurement phase of the calibration.

**Logic:**

1. **If valve is fully open** (`ZSO_10_302` true):
   - Advances to STATE 40 (calculation step)

2. **If valve is not fully open** (`ZSO_10_302` false):
   - Energizes `MOV_10_302_OP` (open output)
   - Runs `DEV_Stroke_Timer` (measuring travel time)
   - Runs `DEV_Timeout_Timer` (safety timeout)

**Tags Involved:**
- `ZSO_10_302`: Bypass valve fully open limit switch input
- `MOV_10_302_OP`: Output to open the bypass valve
- `DEV_Stroke_Timer`: Accumulates the stroke time in milliseconds
- `DEV_Timeout_Timer`: Safety timeout timer

**Safety Feature:** If the timeout expires before the open limit is reached, the fault logic (Rung 9) triggers and moves to STATE 99.

---

### STATE 40: CALCULATE & COMPLETE
**Rung 5**

**Condition:** `DEV_Cal_Step = 40` AND `DEV_Stroke_Timer.ACC > 0`

**Operation:**
With the full stroke time measured, the routine calculates the position increment value.

**Calculation:**
```
Bypass_Position_Increment = 5,000,000 / DEV_Stroke_Timer.ACC
```

**Explanation:**
- The numerator value `5,000,000` represents a normalized full-scale position value
- `DEV_Stroke_Timer.ACC` contains the measured stroke time in milliseconds
- The result is the increment value per millisecond of travel
- This value is used by the position tracking logic to update `Bypass_Position_Counter` during valve movements

**Actions:**
- Performs the division calculation
- Sets `DEV_Cal_Complete` flag (signals successful completion)
- Advances to STATE 50

**Tags Involved:**
- `DEV_Stroke_Timer.ACC`: Accumulated stroke time (milliseconds)
- `Bypass_Position_Increment`: Calculated position increment (output)
- `DEV_Cal_Complete`: Completion flag

---

### STATE 50: CLEANUP
**Rung 6**

**Condition:** `DEV_Cal_Step = 50`

**Operation:**
Final state that resets all calibration flags and returns to idle state.

**Actions:**
- Clears `DEV_Cal_Complete` flag (unlatch)
- Clears `DEV_Cal_In_Progress` flag (unlatch)
- Returns to STATE 0 (Idle)

**Tags Involved:**
- `DEV_Cal_Complete`: Completion flag
- `DEV_Cal_In_Progress`: Active calibration flag

---

## Abort Logic

### ABORT HANDLING
**Rung 7**

**Condition:** `DEV_Cal_Step = 10, 20, or 30` AND `Dev_Abort_CMD` activated

**Operation:**
The operator can abort the calibration during any active movement or preparation step by activating the abort command.

**Actions:**
- Immediately advances to STATE 98 (Aborted state)

**Tags Involved:**
- `Dev_Abort_CMD`: HMI abort button
- `DEV_ONS[2]`: One-shot for abort command

---

### STATE 98: ABORTED
**Rung 8**

**Condition:** `DEV_Cal_Step = 98`

**Operation:**
The calibration sequence was stopped by user request. This state safely shuts down all outputs and waits for the operator to release the enable button before allowing a return to idle.

**Actions:**
- Sets `DEV_Cal_Aborted` flag
- Clears `DEV_Cal_In_Progress` flag
- De-energizes `MOV_10_302_OP` (stop opening)
- De-energizes `MOV_10_302_CL` (stop closing)
- When `DEV_Calibrate_Enable` is released (turned off):
  - Clears `DEV_Cal_Aborted` flag
  - Returns to STATE 0 (Idle)

**Tags Involved:**
- `DEV_Cal_Aborted`: Abort status flag
- `DEV_Cal_In_Progress`: Active calibration flag
- `MOV_10_302_OP`: Open output
- `MOV_10_302_CL`: Close output
- `DEV_Calibrate_Enable`: Must be released to reset

---

## Fault Logic

### TIMEOUT FAULT DETECTION
**Rung 9**

**Condition:** `DEV_Cal_Step = 10 or 30` AND `DEV_Timeout_Timer.DN` is true

**Operation:**
If the valve fails to reach the commanded position (closed or open) before the timeout expires, the sequence is faulted.

**Actions:**
- Sets `DEV_Cal_Failed` flag
- Advances to STATE 99 (Faulted state)

**Tags Involved:**
- `DEV_Timeout_Timer.DN`: Timeout expired
- `DEV_Cal_Failed`: Fault status flag

**Common Fault Causes:**
- Valve actuator malfunction
- Limit switch failure or misalignment
- Mechanical obstruction preventing valve movement
- Insufficient air pressure (if pneumatic actuator)

---

### STATE 99: FAULTED
**Rung 10**

**Condition:** `DEV_Cal_Step = 99`

**Operation:**
The calibration sequence has faulted due to a timeout. All valve outputs are stopped, and the routine waits for the operator to acknowledge and reset.

**Actions:**
- Clears `DEV_Cal_In_Progress` flag
- De-energizes `MOV_10_302_OP` (stop opening)
- De-energizes `MOV_10_302_CL` (stop closing)
- When `DEV_Calibrate_Enable` is turned off (cycled):
  - Clears `DEV_Cal_Failed` flag
  - Returns to STATE 0 (Idle)

**Tags Involved:**
- `DEV_Cal_Failed`: Fault status flag
- `DEV_Cal_In_Progress`: Active calibration flag
- `MOV_10_302_OP`: Open output
- `MOV_10_302_CL`: Close output
- `DEV_Calibrate_Enable`: Must be cycled to reset
- `DEV_ONS[3]`: One-shot for reset

**Recovery:** The operator must investigate the cause of the timeout before attempting another calibration.

---

## State Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         STATE 0: IDLE                            │
│                     (Waiting for operator)                       │
└──────────────────────────────┬──────────────────────────────────┘
                               │ DEV_Calibrate_Enable pressed
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│              STATE 5: AWAITING CONFIRMATION                      │
│                (10-second confirmation window)                   │
└──────┬──────────────────────────────────────────────────┬───────┘
       │ DEV_Confirm_CMD pressed                          │ Timeout (10s)
       ▼                                                   ▼
┌─────────────────────────────────────────┐         [Return to STATE 0]
│    STATE 10: DRIVE TO CLOSE             │
│ (Establish zero reference - closed)     │
└──────┬──────────────────────────────────┘
       │ ZSC_10_302 (Closed limit reached)
       ▼
┌─────────────────────────────────────────┐
│   STATE 20: RESET AND PREPARE           │
│  (Clear timers and position counter)    │
└──────┬──────────────────────────────────┘
       │ Immediate
       ▼
┌─────────────────────────────────────────┐
│  STATE 30: DRIVE TO OPEN & MEASURE      │
│  (Full stroke - measure travel time)    │
└──────┬──────────────────────────────────┘
       │ ZSO_10_302 (Open limit reached)
       ▼
┌─────────────────────────────────────────┐
│   STATE 40: CALCULATE & COMPLETE        │
│ (Calculate position increment value)    │
└──────┬──────────────────────────────────┘
       │ Calculation complete
       ▼
┌─────────────────────────────────────────┐
│        STATE 50: CLEANUP                │
│    (Reset flags, return to idle)        │
└──────┬──────────────────────────────────┘
       │
       └──────────────────► [Return to STATE 0]


           ┌──── FAULT/ABORT PATHS ────┐

Dev_Abort_CMD ──────────► STATE 98: ABORTED
(from STATE 10, 20, 30)   (Wait for enable release)
                                 │
                                 └──► [Return to STATE 0]

Timeout ─────────────────► STATE 99: FAULTED
(from STATE 10 or 30)      (Wait for enable cycle)
                                 │
                                 └──► [Return to STATE 0]
```

---

## Key Tags Summary

### Input Tags
- `ZSC_10_302`: Bypass valve fully closed limit switch
- `ZSO_10_302`: Bypass valve fully open limit switch
- `DEV_Calibrate_Enable`: HMI button to start calibration
- `DEV_Confirm_CMD`: HMI button to confirm calibration start
- `Dev_Abort_CMD`: HMI button to abort calibration

### Output Tags
- `MOV_10_302_CL`: Output command to close bypass valve
- `MOV_10_302_OP`: Output command to open bypass valve

### Status/Flag Tags
- `DEV_Cal_Step`: Current state number (0, 5, 10, 20, 30, 40, 50, 98, 99)
- `DEV_Cal_In_Progress`: Calibration is actively running
- `DEV_Awaiting_Confirm`: Waiting for operator confirmation
- `DEV_Cal_Complete`: Calibration successfully completed (pulsed)
- `DEV_Cal_Aborted`: Calibration was aborted by operator
- `DEV_Cal_Failed`: Calibration failed due to timeout

### Measurement Tags
- `Bypass_Position_Counter`: Raw position counter value (reset to 0 at closed)
- `Bypass_Position_Increment`: Calculated increment value (calibration result)
- `DEV_Stroke_Timer`: Measures the time to travel from closed to open
- `DEV_Timeout_Timer`: Safety timeout for valve movements
- `DEV_Confirmation_Timer`: 10-second confirmation window timer

### Working Tags
- `DEV_ONS[0..31]`: Array of one-shot bits for edge detection

---

## Timing and Preset Values

### Timers
- **DEV_Confirmation_Timer**: Preset = 10,000 ms (10 seconds)
  - Provides operator confirmation window
  
- **DEV_Stroke_Timer**: Preset = Not preset (counts up)
  - Measures actual stroke time from closed to open
  
- **DEV_Timeout_Timer**: Preset = Not visible in export (set elsewhere)
  - Safety timeout for valve movements

### Position Calculation
- **Numerator**: 5,000,000 (normalized full-scale position)
- **Formula**: `Position_Increment = 5,000,000 / Stroke_Time_ms`
- **Initial Position Counter**: 3,300,000 (current value before calibration)

---

## Operational Sequence Summary

1. **Initiation**: Operator presses 'Calibrate' button (`DEV_Calibrate_Enable`)
2. **Confirmation**: System waits 10 seconds for 'Confirm' button press
3. **Close Stroke**: Valve drives to fully closed position (zero reference)
4. **Reset**: Position counter and stroke timer are cleared
5. **Open Stroke**: Valve drives to fully open while stroke time is measured
6. **Calculation**: Position increment value is calculated based on stroke time
7. **Completion**: Flags are reset and system returns to idle

**Total Duration**: Approximately 10 seconds (confirmation) + valve close time + valve open time + calculation time

---

## Safety Features

1. **Confirmation Window**: 10-second window prevents accidental calibration starts
2. **Timeout Protection**: Movement timeouts prevent infinite wait on stuck valves
3. **Abort Capability**: Operator can stop calibration at any time
4. **Interlocked Outputs**: Only one direction command active at a time
5. **State-Based Control**: Clear state machine prevents ambiguous conditions
6. **Limit Switch Verification**: Requires actual limit switch activation (not time-based)
7. **Enable Interlock**: Faulted and aborted states require enable cycling to reset

---

## HMI Requirements

The HMI should display:
- Current calibration step number (`DEV_Cal_Step`)
- Status flags (In Progress, Complete, Aborted, Failed)
- Measured stroke time (`DEV_Stroke_Timer.ACC`)
- Calculated position increment (`Bypass_Position_Increment`)
- Confirmation countdown (10 - `DEV_Confirmation_Timer.ACC` / 1000)

The HMI should provide:
- 'Calibrate' button (`DEV_Calibrate_Enable`)
- 'Confirm' button (`DEV_Confirm_CMD`)
- 'Abort' button (`Dev_Abort_CMD`)

---

## Maintenance Notes

### When to Calibrate
- After valve actuator maintenance or replacement
- After limit switch adjustment or replacement
- If position feedback appears inaccurate
- After any mechanical work on the valve assembly
- Periodically as part of preventive maintenance schedule

### Calibration Prerequisites
- Valve must be in manual/local control mode
- All interlocks must be cleared
- Valve must be free to move through full travel
- Limit switches must be properly adjusted and functional
- Sufficient motive power (air pressure, electrical power, etc.)

### Troubleshooting
- **Fault at STATE 10**: Check closed limit switch, valve can't reach closed position
- **Fault at STATE 30**: Check open limit switch, valve can't reach open position
- **Invalid Position Increment**: Check if stroke time is reasonable (not zero, not excessive)
- **Confirmation Timeout**: Operator took too long to press confirm button

---

## Document Information
- **Routine Name**: Valve_Position_Calibrate_Routine_RLL_Nov6_R0
- **File**: Valve_Position_Calibrate_Routine_RLL_Nov6_R0.L5X
- **Valve**: MOV-10-302 (Bypass Valve)
- **Documentation Date**: November 6, 2025
- **Logic Version**: Nov6_R0