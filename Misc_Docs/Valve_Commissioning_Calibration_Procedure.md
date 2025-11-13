# Valve Position Calibration - Commissioning Procedure

**Project:** Arrowsmith Dam PLC  
**Document Date:** November 13, 2025  
**Revision:** 1.0  
**Valves Covered:** Main Valve (MOV-10-300), Bypass Valve (MOV-10-302), Siphon Valve (MOV-10-301)

---

## Purpose

This document provides step-by-step procedures for commissioning engineers to calibrate the valve position feedback system during initial site commissioning. The calibration establishes accurate position scaling and measures actual valve stroke times under various operating conditions.

---

## Overview of Position Calibration System

The valve position simulation uses an **automatic calibration feature** integrated into the `Scaling_Averaging` function block routine:

- **When ZSC (Fully Closed) limit switch activates** → The current `Position_Counter` value is captured and stored in `InRawMin` (becomes the 0% reference)
- **When ZSO (Fully Open) limit switch activates** → The current `Position_Counter` value is captured and stored in `InRawMax` (becomes the 100% reference)

The scaling function then uses these captured min/max values to calculate the position percentage. This auto-calibration ensures the position scaling adapts to the actual position counter values at the limit switches.

---

## ⚠️ CRITICAL WARNING - Zero Range Trap

**DO NOT test limit switches without moving the valve first!**

If you test the ZSC and ZSO limit switches by manually triggering them (e.g., with a magnet or by hand) **without physically moving the valve**, the auto-calibration will capture the same `Position_Counter` value for both `InRawMin` and `InRawMax`. This creates a **zero range** condition where:

- The minimum and maximum values are identical
- Position percentage cannot be calculated correctly (division by zero or invalid range)
- Position tracking will not work correctly
- Position percentage will be stuck or erratic
- The system will need to be re-initialized

**Always follow the proper initialization sequence in Section 3.**

---

## Valve Information

Complete one set of procedures for each valve:

| **Valve** | **Tag Prefix** | **ZSC Tag** | **ZSO Tag** | **Position Counter Tag** |
|-----------|---------------|-------------|-------------|--------------------------|
| Main Valve | `Main_` | `ZSC_10_300` | `ZSO_10_300` | `Main_Position_Counter` |
| Bypass Valve | `Bypass_` | `ZSC_10_302` | `ZSO_10_302` | `Bypass_Position_Counter` |
| Siphon Valve | `Siphon_` | `ZSC_10_301` | `ZSO_10_301` | `Siphon_Position_Counter` |

---

## Section 1: Limit Switch Functional Test (Optional)

**⚠️ WARNING:** If you perform this test, you MUST follow Section 3 (Initialization) afterward to avoid the zero range trap.

This optional test verifies limit switch wiring without valve movement:

### For ZSC (Closed Limit):
1. Manually activate ZSC switch (magnet, hand actuation, etc.)
2. Verify PLC tag `ZSC_10_XXX` changes to TRUE
3. Release switch
4. Verify tag returns to FALSE

### For ZSO (Open Limit):
1. Manually activate ZSO switch
2. Verify PLC tag `ZSO_10_XXX` changes to TRUE
3. Release switch
4. Verify tag returns to FALSE

**ZSC Test Result:** ☐ PASS ☐ FAIL  
**ZSO Test Result:** ☐ PASS ☐ FAIL

**If you performed this test, you MUST now proceed directly to Section 3 before any valve movement.**

---

## Section 2: Valve Movement Test

Verify the valve can be controlled from the HMI:

1. From HMI, issue **Open** command
2. Verify valve begins opening
3. Stop command after 2-3 seconds
4. From HMI, issue **Close** command
5. Verify valve begins closing
6. Stop command after 2-3 seconds

**Valve Control Test:** ☐ PASS ☐ FAIL

---

## Section 3: Position Counter Initialization (MANDATORY)

This procedure sets up the position counter correctly before enabling auto-calibration.

### Step 1: Move Valve to Mid-Stroke
1. From HMI, command valve to approximately **50% open** position
2. Verify valve is physically near mid-stroke (visually or by indicator)
3. Stop the valve

### Step 2: Set Position Counter to Mid-Range
1. In PLC, locate tag: `[Valve]_Position_Counter` (e.g., `Main_Position_Counter`)
2. **Manually write value: 2,500,000** to this tag
3. Verify HMI shows approximately **50%** position

**Position Counter Initialized:** ☐ YES **Date/Time:** _________________

### Step 3: Enable Auto-Calibration at Closed Limit
1. From HMI, command valve to **CLOSE**
2. Allow valve to travel until **ZSC activates** (valve fully closed)
3. The system will automatically capture the current `Position_Counter` value into `InRawMin`
4. Verify HMI shows **0%** position

**ZSC Auto-Cal Result:** 
- InRawMin (captured value) = __________ 
- Position Counter = __________
- HMI Display = __________% (should be 0%)

### Step 4: Enable Auto-Calibration at Open Limit
1. From HMI, command valve to **OPEN**
2. Allow valve to travel until **ZSO activates** (valve fully open)
3. The system will automatically capture the current `Position_Counter` value into `InRawMax`
4. Verify HMI shows **100%** position

**ZSO Auto-Cal Result:**
- InRawMax (captured value) = __________
- Position Counter = __________
- HMI Display = __________% (should be 100%)

**Initialization Complete:** ☐ YES

---

## Section 4: Stroke Time Measurements

### Purpose
Measure actual valve stroke times under different operating conditions to:
- Understand valve performance characteristics
- Quantify stiction effects in incremental vs. continuous operation
- Compare opening vs. closing performance
- Establish baseline data for the `Full_Stroke_Time_SP` setpoint

### 4.1 Continuous Full Stroke Tests

#### Test 1: ZSO → ZSC (Open to Closed, Continuous)

**Starting Condition:** Valve at ZSO (fully open, 100%)

1. Start stopwatch
2. Issue **CLOSE** command from HMI
3. Allow valve to travel continuously until ZSC activates
4. Stop stopwatch when ZSC activates
5. Record time below

**Measured Time (ZSO → ZSC):** __________ seconds

**Observations/Notes:**
_______________________________________________________________________________
_______________________________________________________________________________

---

#### Test 2: ZSC → ZSO (Closed to Open, Continuous)

**Starting Condition:** Valve at ZSC (fully closed, 0%)

1. Start stopwatch
2. Issue **OPEN** command from HMI
3. Allow valve to travel continuously until ZSO activates
4. Stop stopwatch when ZSO activates
5. Record time below

**Measured Time (ZSC → ZSO):** __________ seconds

**Observations/Notes:**
_______________________________________________________________________________
_______________________________________________________________________________

---

### 4.2 Incremental Stroke Tests (3-Second Pulses)

These tests simulate typical operational moves where the valve is commanded in short increments rather than full strokes. Stiction may cause longer total times compared to continuous operation.

#### Test 3: ZSO → ZSC (Open to Closed, 3-Second Increments)

**Starting Condition:** Valve at ZSO (fully open, 100%)

**Procedure:**
1. Start overall stopwatch
2. Issue **CLOSE** command for **3 seconds**, then STOP
3. Wait 2 seconds (valve settles)
4. Repeat step 2-3 until ZSC activates
5. Stop overall stopwatch when ZSC activates
6. Count total number of 3-second pulses required

**Results:**
- **Total Time (start to ZSC activation):** __________ seconds
- **Number of 3-second pulses:** __________
- **Theoretical time (# pulses × 3 sec):** __________ seconds
- **Dead time due to stiction (Total - Theoretical):** __________ seconds

**Observations/Notes:**
_______________________________________________________________________________
_______________________________________________________________________________

---

#### Test 4: ZSC → ZSO (Closed to Open, 3-Second Increments)

**Starting Condition:** Valve at ZSC (fully closed, 0%)

**Procedure:**
1. Start overall stopwatch
2. Issue **OPEN** command for **3 seconds**, then STOP
3. Wait 2 seconds (valve settles)
4. Repeat step 2-3 until ZSO activates
5. Stop overall stopwatch when ZSO activates
6. Count total number of 3-second pulses required

**Results:**
- **Total Time (start to ZSO activation):** __________ seconds
- **Number of 3-second pulses:** __________
- **Theoretical time (# pulses × 3 sec):** __________ seconds
- **Dead time due to stiction (Total - Theoretical):** __________ seconds

**Observations/Notes:**
_______________________________________________________________________________
_______________________________________________________________________________

---

## Section 5: Data Analysis & Findings

### 5.1 Stroke Time Comparison

| **Test** | **Measured Time** | **Notes** |
|----------|-------------------|-----------|
| ZSO → ZSC (Continuous) | __________ sec | Closing, no stiction |
| ZSC → ZSO (Continuous) | __________ sec | Opening, no stiction |
| ZSO → ZSC (Incremental) | __________ sec | Closing with stiction |
| ZSC → ZSO (Incremental) | __________ sec | Opening with stiction |

### 5.2 Stiction Analysis

**Closing Direction:**
- Stiction penalty = __________ seconds (Incremental - Continuous)
- Percent increase = __________% 

**Opening Direction:**
- Stiction penalty = __________ seconds (Incremental - Continuous)
- Percent increase = __________%

### 5.3 Directional Comparison

**Continuous Operation:**
- Opening vs Closing difference = __________ seconds
- Faster direction: ☐ Opening ☐ Closing

**Incremental Operation:**
- Opening vs Closing difference = __________ seconds
- Faster direction: ☐ Opening ☐ Closing

---

## Section 6: Set Full Stroke Time Setpoint

Based on the measured data, set the `Full_Stroke_Time_SP` parameter for normal operations.

**Recommended Value Selection:**

**Option 1 (Preferred if stiction is significant):**  
If incremental times are significantly longer than continuous (>10% difference), use the **average of the two incremental stroke times** (Tests 3 & 4) since they better represent typical 3-second pulse operation:
```
Full_Stroke_Time_SP = (ZSO→ZSC incremental + ZSC→ZSO incremental) / 2
Full_Stroke_Time_SP = (_______ + _______) / 2 = _______ seconds
```

**Option 2 (Preferred if times are consistent):**  
If all four times are relatively close (incremental vs continuous difference <10%), use the **average of all four measurements** for the most statistically robust value:
```
Full_Stroke_Time_SP = (Test 1 + Test 2 + Test 3 + Test 4) / 4
Full_Stroke_Time_SP = (_____ + _____ + _____ + _____) / 4 = _______ seconds
```

**Option 3 (Conservative baseline only):**  
Use only the continuous times if you want the fastest theoretical response (not recommended for typical operation):
```
Full_Stroke_Time_SP = (ZSO→ZSC continuous + ZSC→ZSO continuous) / 2
Full_Stroke_Time_SP = (_______ + _______) / 2 = _______ seconds
```

**Action:**
1. In PLC, locate tag: `[Valve]_Full_Stroke_Time_SP`
2. Write calculated value: __________ seconds
3. Verify value is accepted by PLC

**Full_Stroke_Time_SP Set:** ☐ YES **Value:** __________ seconds  
**Method Used:** ☐ Option 1 ☐ Option 2 ☐ Option 3

---

## Section 7: Position Tracking Verification

Verify the position simulation is working correctly with the calibrated values.

### Test 1: Full Stroke Position Tracking
1. Command valve to **CLOSE** from fully open
2. Watch HMI position display during travel
3. Verify position counts down smoothly from 100% to 0%
4. Verify ZSC activation sets position to exactly 0%

**Result:** ☐ PASS ☐ FAIL

### Test 2: Mid-Range Position Accuracy
1. Command valve to **OPEN** from fully closed
2. After approximately **40-50% of the measured full stroke time**, **STOP** valve
   - Example: If full stroke = 60 seconds, stop after 25-30 seconds
3. Note displayed position: __________%
4. Estimate actual physical position (visual/indicator): __________%
5. Calculate error: __________ % (displayed - actual)

**Acceptable if error is within ±5%:** ☐ PASS ☐ FAIL

### Test 3: Reverse Direction Tracking
1. From mid-range position, command valve to **CLOSE**
2. Watch position count down
3. Stop after 5-10 seconds
4. Command valve to **OPEN**
5. Watch position count up
6. Verify smooth bidirectional tracking

**Result:** ☐ PASS ☐ FAIL

---

## Section 8: Acceptance Criteria

All of the following must be satisfied for commissioning acceptance:

- [ ] ZSC and ZSO limit switches trigger correctly
- [ ] InRawMin is captured when ZSC activates
- [ ] InRawMax is captured when ZSO activates
- [ ] InRawMax is greater than InRawMin (valid range established)
- [ ] HMI displays 0% when valve is at ZSC
- [ ] HMI displays 100% when valve is at ZSO
- [ ] Full stroke time measured and recorded
- [ ] `Full_Stroke_Time_SP` parameter set in PLC
- [ ] Position tracking verified in both directions
- [ ] Mid-range position accuracy within ±5%
- [ ] All test data recorded in this document

**Overall Commissioning Result:** ☐ PASS ☐ FAIL

---

## Section 9: Troubleshooting Guide

| **Problem** | **Possible Cause** | **Solution** |
|-------------|-------------------|--------------|
| Position stuck at 0% or 100% | Zero range trap - InRawMin equals InRawMax | Re-initialize per Section 3 |
| Position doesn't change during movement | `Full_Stroke_Time_SP` is zero or very large | Verify `Full_Stroke_Time_SP` is set correctly |
| Position doesn't show 0% at ZSC | InRawMin not captured or incorrect | Re-run Section 3, Step 3 |
| Position doesn't show 100% at ZSO | InRawMax not captured or incorrect | Re-run Section 3, Step 4 |
| Position drifts over time | Timing mismatch | Re-measure stroke time and update `Full_Stroke_Time_SP` |
| Large error at mid-range | Incorrect `Full_Stroke_Time_SP` | Re-calibrate stroke time |

---

## Appendix A: Key PLC Tags Reference

### Main Valve (MOV-10-300)
- `Main_Position_Counter` - Raw position counter (increments/decrements during valve movement)
- `Main_Position_Percent` - Position percentage (0 to 100%)
- `Main_InRawMin` - Minimum raw value (captured at ZSC, becomes 0% reference)
- `Main_InRawMax` - Maximum raw value (captured at ZSO, becomes 100% reference)
- `Main_Full_Stroke_Time_SP` - Full stroke time setpoint (seconds)
- `ZSC_10_300` - Fully closed limit switch
- `ZSO_10_300` - Fully open limit switch

### Bypass Valve (MOV-10-302)
- `Bypass_Position_Counter` - Raw position counter (increments/decrements during valve movement)
- `Bypass_Position_Percent` - Position percentage (0 to 100%)
- `Bypass_InRawMin` - Minimum raw value (captured at ZSC, becomes 0% reference)
- `Bypass_InRawMax` - Maximum raw value (captured at ZSO, becomes 100% reference)
- `Bypass_Full_Stroke_Time_SP` - Full stroke time setpoint (seconds)
- `ZSC_10_302` - Fully closed limit switch
- `ZSO_10_302` - Fully open limit switch

### Siphon Valve (MOV-10-301)
- `Siphon_Position_Counter` - Raw position counter (increments/decrements during valve movement)
- `Siphon_Position_Percent` - Position percentage (0 to 100%)
- `Siphon_InRawMin` - Minimum raw value (captured at ZSC, becomes 0% reference)
- `Siphon_InRawMax` - Maximum raw value (captured at ZSO, becomes 100% reference)
- `Siphon_Full_Stroke_Time_SP` - Full stroke time setpoint (seconds)
- `ZSC_10_301` - Fully closed limit switch
- `ZSO_10_301` - Fully open limit switch

---

## Appendix B: Understanding Stiction Effects

**Stiction** (static friction) is the resistance a valve must overcome to start moving from a stopped position. This is typically higher than the friction during continuous movement.

### Why Measure Incremental Movement?
During normal operation, valves are often commanded in short pulses (e.g., 3-second moves) rather than continuous full strokes. Each time the valve stops and restarts, stiction must be overcome again, adding "dead time" to the total travel.

### Expected Behavior:
- **Incremental times > Continuous times** - Normal, indicates stiction is present
- **Opening vs. Closing differences** - May vary based on actuator design and valve orientation
- **Typical stiction penalty** - 10-30% increase in total time for incremental vs. continuous

