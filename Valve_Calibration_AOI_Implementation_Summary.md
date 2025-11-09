# Valve Position Calibration AOI - Implementation Summary

**Date:** November 6, 2025  
**Project:** Arrowsmith Dam PLC  
**Valve:** MOV-10-302 (Bypass Valve) - Initial Implementation  
**Future Valves:** Main Valve, Siphon Valve

---

## Overview

A new **Add-On Instruction (AOI)** named `Valve_Position_Calibration` has been created to provide semi-automatic valve position calibration for motorized valves. This AOI can be reused for multiple valves (Bypass, Main, Siphon) with minimal configuration.

---

## What Was Implemented

### 1. **AOI Creation: `Valve_Position_Calibration`**

The AOI provides a complete semi-automatic calibration sequence with user confirmations at key steps.

#### **AOI Parameters:**

**INPUT Parameters:**
- `Enable` (BOOL) - Start calibration (HMI button)
- `Confirm_CMD` (BOOL) - User confirmation button
- `Abort_CMD` (BOOL) - User abort button
- `ZSC` (BOOL) - Valve fully closed limit switch
- `ZSO` (BOOL) - Valve fully open limit switch
- `Timeout_Preset` (DINT) - Movement timeout in ms (default: 60000 = 1 minute)
- `Delay_Preset` (DINT) - Start delay in ms (default: 500ms, adjustable on-site)
- `Confirmation_Preset` (DINT) - Confirmation timeout in ms (default: 10000 = 10 seconds)

**OUTPUT Parameters:**
- `Valve_Open_CMD` (BOOL) - Command to open valve
- `Valve_Close_CMD` (BOOL) - Command to close valve
- `Cal_In_Progress` (BOOL) - Calibration is actively running
- `Cal_Complete` (BOOL) - Successfully completed
- `Cal_Aborted` (BOOL) - User aborted
- `Cal_Failed` (BOOL) - Timed out or faulted
- `Awaiting_Confirm` (BOOL) - Waiting for user confirmation
- `Cal_Step` (DINT) - Current state number (for HMI display)

**INOUT Parameters:**
- `Position_Counter` (DINT) - Real-time position counter
- `InRawMin` (DINT) - Minimum raw position value (set at closed)
- `InRawMax` (DINT) - Maximum raw position value (set at open)
- `Full_Stroke_Time_SP` (REAL) - Measured stroke time in seconds
- `Position_Percent` (DINT) - Position percentage for display

**LOCAL Tags (Internal to AOI):**
- `Confirmation_Timer` (TIMER)
- `Timeout_Timer` (TIMER)
- `Delay_Timer` (TIMER)
- `Stroke_Timer` (TIMER)
- `ONS[32]` (BOOL array)
- `Stroke_Time_Sec` (REAL) - Temporary conversion variable

---

## Calibration Sequence Flow

### **Semi-Automatic Operation with User Confirmations**

```
STATE 0: IDLE
    ↓ (Operator presses Enable)
    
STATE 5: AWAITING CONFIRMATION (10 seconds)
    ↓ (Operator presses Confirm within 10s)
    ↓ (Timeout → return to STATE 0)
    
STATE 10: CLOSING (60 second timeout)
    - Command valve to close
    - Wait for ZSC limit switch
    ↓ (ZSC reached)
    
STATE 15: CLOSED CONFIRMED
    - Set InRawMin = 0
    - Set Position_Counter = 0
    - STOP and WAIT for operator confirmation
    ↓ (Operator presses Confirm)
    
STATE 20: DELAY BEFORE OPENING (500ms adjustable)
    - Compensate for valve stickage
    - Output energized but timer not started yet
    ↓ (500ms elapsed)
    
STATE 25: OPENING (60 second timeout)
    - Command valve to open
    - Start measuring stroke time
    - Wait for ZSO limit switch
    ↓ (ZSO reached)
    
STATE 30: OPEN CONFIRMED
    - Set InRawMax = 5,000,000
    - Set Position_Counter = 5,000,000
    - Save stroke time: Full_Stroke_Time_SP = Stroke_Timer.ACC / 1000.0 seconds
    - Position_Percent should show 100%
    - STOP and WAIT for operator confirmation
    ↓ (Operator presses Confirm)
    
STATE 31: WAITING FOR CONFIRMATION TO MOVE TO 50%
    ↓ (Operator presses Confirm)
    
STATE 35: MOVING TO 50%
    - Automatically move valve to 2,500,000 ± 50,000 (±1%)
    - If Position < 2,450,000 → Open valve
    - If Position > 2,550,000 → Close valve
    - When 2,450,000 ≤ Position ≤ 2,550,000 → Stop
    ↓ (50% reached)
    
STATE 40: AT 50% COMPLETE
    - Set Cal_Complete flag
    - Operator visually verifies valve is at 50%
    - Auto-advance to cleanup (no confirmation needed)
    ↓
    
STATE 50: CLEANUP
    - Clear flags
    - Return to STATE 0 (IDLE)

--- ABORT PATH ---
STATE 98: ABORTED
    - Available from states 10, 15, 20, 25, 30, 31, 35
    - Stop all outputs
    - Wait for Enable to be released
    - Return to STATE 0

--- FAULT PATH ---
STATE 99: FAULTED
    - Triggered by 60-second timeout in STATE 10 or 25
    - Stop all outputs
    - Wait for Enable to be cycled off/on
    - Return to STATE 0
```

---

## Key Features

### **1. Semi-Automatic Operation**
- **3 User Confirmations Required:**
  1. Initial confirmation to start (10 seconds)
  2. Confirmation after closed position set
  3. Confirmation after open position set (to move to 50%)
- **Auto-Complete:** After 50% verification, no user input needed

### **2. Safety Features**
- **Timeouts:** 60-second (1 minute) timeout for valve movements
- **Abort Capability:** User can abort at any active step
- **Confirmation Window:** 10-second window prevents accidental starts
- **Delay Timer:** 500ms adjustable delay before opening compensates for valve stickage

### **3. Accurate Measurements**
- **Zero Reference:** Established at closed position (ZSC)
- **Stroke Time:** Measured from start of open command (after delay) to ZSO reached
- **Stroke Time Saved:** Converted to seconds and stored in `Full_Stroke_Time_SP`
- **Min/Max Values:** InRawMin = 0, InRawMax = 5,000,000
- **50% Verification:** Automatic positioning to 2,500,000 with ±50,000 tolerance

### **4. Flexibility**
- **Adjustable Timeouts:** All timers can be adjusted via AOI parameters
- **Reusable:** Same AOI for Bypass, Main, and Siphon valves
- **On-Site Tuning:** Delay timer can be adjusted during commissioning

---

## Files Modified

### **1. Arrowsmith_Dam_Nov6_R0.L5X (Main Controller)**
- **Added:** `Valve_Position_Calibration` AOI definition in `AddOnInstructionDefinitions` section
- **Added:** AOI instance call in `MainRoutine` (Rung 8) for Bypass valve
- **Updated:** `Valve_Position_Calibrate` routine with new semi-automatic logic (for reference/backup)

### **2. Valve_Position_Calibrate_Routine_RLL_Nov6_R0.L5X (Standalone)**
- **Added:** New tags: `Bypass_InRawMin`, `Bypass_InRawMax`, `Bypass_Full_Stroke_Time_SP`, `Bypass_Position_Percent`, `DEV_Delay_Timer`
- **Updated:** All rungs to match new semi-automatic logic
- **Updated:** Comments to reflect new state flow

### **3. Documentation**
- **Created:** This summary file
- **To Update:** `Valve_Position_Calibrate_Routine_Operation.md` (next step)

---

## AOI Instance for Bypass Valve

### **Rung 8 in MainRoutine:**

```ladder
Valve_Position_Calibration(
    DEV_Calibrate_Enable,              // Enable input
    DEV_Confirm_CMD,                   // Confirm button
    Dev_Abort_CMD,                     // Abort button
    ZSC_10_302,                        // Closed limit switch
    ZSO_10_302,                        // Open limit switch
    60000,                             // Timeout: 60 seconds
    500,                               // Delay: 500ms
    10000,                             // Confirmation: 10 seconds
    MOV_10_302_OP,                     // Open command output
    MOV_10_302_CL,                     // Close command output
    DEV_Cal_In_Progress,               // In progress flag
    DEV_Cal_Complete,                  // Complete flag
    DEV_Cal_Aborted,                   // Aborted flag
    DEV_Cal_Failed,                    // Failed flag
    DEV_Awaiting_Confirm,              // Awaiting confirmation flag
    DEV_Cal_Step,                      // Current step
    Bypass_Position_Counter,           // Position counter (INOUT)
    Bypass_InRawMin,                   // Min raw value (INOUT)
    Bypass_InRawMax,                   // Max raw value (INOUT)
    Bypass_Full_Stroke_Time_SP,        // Stroke time (INOUT)
    Bypass_Position_Percent            // Position % (INOUT)
);
```

---

## Future Implementation for Main & Siphon Valves

When ready to add calibration for Main and Siphon valves, simply add more AOI instances:

### **For Main Valve:**
```ladder
Valve_Position_Calibration(
    DEV_Calibrate_Enable_Main,         // Different enable button
    DEV_Confirm_CMD,                   // Same confirm button (or separate)
    Dev_Abort_CMD,                     // Same abort button (or separate)
    ZSC_10_301,                        // Main valve closed LS
    ZSO_10_301,                        // Main valve open LS
    60000,                             // Timeout
    500,                               // Delay
    10000,                             // Confirmation
    MOV_10_301_OP,                     // Main valve open output
    MOV_10_301_CL,                     // Main valve close output
    DEV_Cal_In_Progress_Main,          // Main valve flags
    DEV_Cal_Complete_Main,
    DEV_Cal_Aborted_Main,
    DEV_Cal_Failed_Main,
    DEV_Awaiting_Confirm_Main,
    DEV_Cal_Step_Main,
    Main_Position_Counter,             // Main valve tags
    Main_InRawMin,
    Main_InRawMax,
    Main_Full_Stroke_Time_SP,
    Main_Position_Percent
);
```

**Repeat for Siphon Valve with appropriate I/O and tags.**

---

## Tag Naming Convention & Control Strategy

### **Hybrid Approach (Selected)**

We are using a **hybrid approach** with shared calibration control tags that work for all valves:

**Shared Control Tags (CAL_...):**
- `CAL_Enable` - Enable/Start calibration
- `CAL_Confirm_CMD` - Confirmation button for all steps
- `CAL_Abort_CMD` - Abort current calibration
- `CAL_Step` - Current calibration step number
- `CAL_In_Progress` - Calibration running flag
- `CAL_Complete` - Calibration success flag
- `CAL_Aborted` - Calibration aborted flag
- `CAL_Failed` - Calibration failed/timeout flag
- `CAL_Awaiting_Confirm` - Waiting for operator confirmation

**Valve Selector:**
- `CAL_Valve_Select` (DINT) - Selects which valve to calibrate:
  - `0` = None (disabled)
  - `1` = Bypass Valve
  - `2` = Main Valve
  - `3` = Siphon Valve

**Implementation:**
- Only **one AOI instance is active** at a time based on `CAL_Valve_Select` value
- Each AOI instance has conditional logic: `EQU(CAL_Valve_Select, 1)` enables Bypass AOI, etc.
- **One HMI calibration screen** works for any valve selected
- Prevents multiple valves from being calibrated simultaneously

**Valve-Specific Tags (remain unique per valve):**
- `Bypass_Position_Counter`, `Main_Position_Counter`, `Siphon_Position_Counter`
- `Bypass_InRawMin`, `Main_InRawMin`, `Siphon_InRawMin`
- `Bypass_InRawMax`, `Main_InRawMax`, `Siphon_InRawMax`
- `Bypass_Full_Stroke_Time_SP`, `Main_Full_Stroke_Time_SP`, `Siphon_Full_Stroke_Time_SP`
- `Bypass_Position_Percent`, `Main_Position_Percent`, `Siphon_Position_Percent`

---

## HMI Requirements

The HMI should provide:

### **Buttons:**
1. **Valve Selector Dropdown** (`CAL_Valve_Select`)
   - Options: None (0), Bypass (1), Main (2), Siphon (3)
   - Disables all AOI instances when set to 0
   - Enables only selected valve's AOI instance

2. **Calibrate/Enable Button** (`CAL_Enable`)
   - Momentary or maintained depending on design
   - Starts the calibration sequence for selected valve

3. **Confirm Button** (`CAL_Confirm_CMD`)
   - Momentary pushbutton
   - Used 3 times during sequence:
     - Initial confirmation (within 10s)
     - After closed position set
     - After open position set (to move to 50%)

4. **Abort Button** (`CAL_Abort_CMD`)
   - Momentary pushbutton
   - Available anytime during active calibration

### **Display Information:**
- `Cal_Step` - Current step number (0, 5, 10, 15, 20, 25, 30, 31, 35, 40, 50, 98, 99)
- `Cal_In_Progress` - Boolean indicator
- `Awaiting_Confirm` - Prompts operator to press Confirm
- `Cal_Complete` - Success indicator
- `Cal_Aborted` - Abort indicator
- `Cal_Failed` - Fault indicator
- `Full_Stroke_Time_SP` - Measured stroke time in seconds
- `Position_Percent` - Current valve position percentage

### **Step-by-Step HMI Prompts:**
- **Step 0:** "Press Calibrate to begin"
- **Step 5:** "Press Confirm within 10 seconds to proceed" (with countdown)
- **Step 10:** "Closing valve... wait for ZSC"
- **Step 15:** "Closed position set. Press Confirm to open valve"
- **Step 20:** "Delaying... (500ms)"
- **Step 25:** "Opening valve and measuring stroke time..."
- **Step 30:** "Open position set. Stroke time: XX.X seconds. Press Confirm to move to 50%"
- **Step 31:** "Waiting for confirmation..."
- **Step 35:** "Moving to 50%..."
- **Step 40:** "Calibration Complete! Valve at 50%. Verify position matches XX%."
- **Step 98:** "Calibration Aborted. Release Enable to reset."
- **Step 99:** "Calibration Failed (Timeout). Cycle Enable to reset."

---

## Testing Checklist

### **Pre-Test:**
- [ ] Valve is free to move full stroke
- [ ] Limit switches (ZSC, ZSO) are functional and properly adjusted
- [ ] Valve actuator has sufficient power/pressure
- [ ] All interlocks are cleared

### **Test Sequence:**
1. [ ] Press Calibrate - Verify Step advances to 5
2. [ ] Wait 10+ seconds without pressing Confirm - Verify timeout returns to Step 0
3. [ ] Press Calibrate again, press Confirm within 10s - Verify Step advances to 10
4. [ ] Verify valve closes, ZSC activates - Verify Step advances to 15
5. [ ] Verify InRawMin = 0, Position_Counter = 0
6. [ ] Press Confirm - Verify Step advances to 20, then 25
7. [ ] Verify 500ms delay occurs before stroke timer starts
8. [ ] Verify valve opens, stroke time is measured
9. [ ] Verify ZSO activates - Verify Step advances to 30
10. [ ] Verify InRawMax = 5,000,000, Position_Counter = 5,000,000
11. [ ] Verify stroke time is saved in Full_Stroke_Time_SP (in seconds)
12. [ ] Press Confirm - Verify Step advances to 35
13. [ ] Verify valve moves to approximately 50% (2,500,000)
14. [ ] Verify Position_Percent shows ~50%
15. [ ] Physically verify valve is at 50% open
16. [ ] Verify Step advances to 40, then 50, then 0 (auto-complete)

### **Abort Test:**
- [ ] Start calibration, abort at Step 10 - Verify State 98, valve stops
- [ ] Start calibration, abort at Step 25 - Verify State 98, valve stops
- [ ] Verify Enable must be released to reset from State 98

### **Fault Test:**
- [ ] Simulate closed LS failure (disconnect or block valve) - Verify 60s timeout → State 99
- [ ] Simulate open LS failure - Verify 60s timeout → State 99
- [ ] Verify Enable must be cycled to reset from State 99

---

## Troubleshooting

| **Issue** | **Possible Cause** | **Solution** |
|-----------|-------------------|-------------|
| Timeout at Step 10 | Valve won't close, ZSC not activating | Check valve actuator, check ZSC alignment/wiring |
| Timeout at Step 25 | Valve won't open, ZSO not activating | Check valve actuator, check ZSO alignment/wiring |
| Confirmation timeout | Operator didn't press Confirm within 10s | Re-start calibration, press Confirm faster |
| Wrong 50% position | Position tracking issue | Check position counter logic in Valves routine |
| Stroke time = 0 | Delay timer or stroke timer issue | Check timer presets and logic in Step 20/25 |
| Can't reset from fault | Enable not cycled properly | Turn Enable off, wait, turn back on |

---

## Advantages of AOI Approach

### **✅ Benefits:**
1. **Single Source of Code:** Fix once, applies to all valves
2. **Consistency:** All valves behave identically
3. **Scalability:** Easy to add Main and Siphon valves
4. **Maintainability:** Changes in one place only
5. **Testing:** Perfect logic on Bypass, then replicate with confidence
6. **Documentation:** Self-documenting through AOI parameter names
7. **Program Size:** Much smaller than three separate routines

### **🎯 Next Steps:**
1. Download to PLC and test Bypass valve calibration
2. Fine-tune delay timer if needed (500ms default)
3. Verify 50% positioning accuracy
4. When satisfied, add Main valve instance
5. Add Siphon valve instance
6. Update HMI screens for all three valves

---

## Version History

| **Version** | **Date** | **Changes** |
|------------|---------|-------------|
| 1.0 | Nov 6, 2025 | Initial AOI implementation for Bypass valve |

---

## Contact & Support

For questions or modifications to this calibration logic, refer to:
- `Valve_Position_Calibration` AOI definition in Arrowsmith_Dam_Nov6_R0.L5X
- This documentation file
- `Valve_Position_Calibrate_Routine_Operation.md` (to be updated)

---

**End of Implementation Summary**
