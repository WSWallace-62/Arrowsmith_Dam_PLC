# Y_Outputs_To_HMI to X_Outputs_to_VTS Conversion

## Description
Complete conversion of the Y_Outputs_To_HMI routine to X_Outputs_to_VTS routine for VTScada data exchange. All tags with _HMI suffix converted to _VTS suffix, and legacy tags without suffix have _VTS added.

## Implementation Date
November 10, 2025

## Source Routine
Y_Outputs_To_HMI

## Destination Routine
X_Outputs_to_VTS

## Conversion Summary
- **Total Rungs:** 56 (0-55)
- **Total _VTS Tags Created:** 47
- **Source Tags:** Unchanged (remain same as HMI routine)
- **Comments:** Updated from "HMI" to "VTS"

---

## Complete Tag List - All _VTS Tags

### Boolean Tags (24 tags)

**New Tag: BOOL**

Tag Name:
```
Analog_OOR_General_Alarm_VTS
```

Tag Comment:
```
Analog Input General Alarm to VTS
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
BatryChgFault_VTS
```

Tag Comment:
```
BATTERY CHARGER FAULT to VTS
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
BatryChgOK_VTS
```

Tag Comment:
```
BATTERY CHARGE OK to VTS
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
BldTempLow_VTS
```

Tag Comment:
```
BUILDING TEMP LOW to VTS
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
BuildingDoorSw_VTS
```

Tag Comment:
```
Building Door Switch to VTS
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
Cal_Required_General_Alarm_VTS
```

Tag Comment:
```
General Alarm status for Calibration required.  Out of range indication to VTS
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
Dam_Level_Alarm_HI_VTS
```

Tag Comment:
```
Geodetic High Level Alarm Status to VTS
```

Initial Value:
```
1
```

---

**New Tag: BOOL**

Tag Name:
```
Dam_Level_Alarm_LO_VTS
```

Tag Comment:
```
Geodetic Low Level Alarm Status to VTS
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
Dam_Level_ROC_Alarm_VTS
```

Tag Comment:
```
Rate of Change Alarm Status to VTS
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
General_Alarm_VTS
```

Tag Comment:
```
General Alarms Status to VTS
```

Initial Value:
```
1
```

---

**New Tag: BOOL**

Tag Name:
```
Genset_Fault_VTS
```

Tag Comment:
```
Genset fault indication for VTS display
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
Genset_LowFuel_VTS
```

Tag Comment:
```
Genset low fuel alarm for VTS display
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
Genset_Running_VTS
```

Tag Comment:
```
Genset Running status to VTS
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
Intruder_Alarm_VTS
```

Tag Comment:
```
Alarm bit indicating an unauthorized building entry to VTS
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
MeterSump_Lockout_Alarm_VTS
```

Tag Comment:
```
Meter Chamber Sump lockout alarm for VTS display
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
RunMeterSumpPmp_VTS
```

Tag Comment:
```
Meter Chamber Sump Pump Running to VTS
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
RunValveSumpPmp_VTS
```

Tag Comment:
```
Valve Chamber Sump Pump Running to VTS
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
ValveSump_Lockout_Alarm_VTS
```

Tag Comment:
```
Valve Chamber Sump lockout alarm for VTS display
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
VTS_Comm_Fault_Latched_VTS
```

Tag Comment:
```
Latched VTScada comm fault, resettable by operator.
```

Initial Value:
```
1
```

---

**New Tag: BOOL**

Tag Name:
```
VTS_Comm_Fault_VTS
```

Tag Comment:
```
Fault bit indicating loss of communication with VTScada.
```

Initial Value:
```
1
```

---

**New Tag: BOOL**

Tag Name:
```
VTS_Main_VTS
```

Tag Comment:
```
AC POWER ON to VTS
```

Initial Value:
```
1
```

---

**New Tag: BOOL**

Tag Name:
```
VTS_MeterSump_Float_VTS
```

Tag Comment:
```
Meter Sump Float Status to VTS
```

Initial Value:
```
0
```

---

**New Tag: BOOL**

Tag Name:
```
VTS_ValveSump_Float_VTS
```

Tag Comment:
```
Valve Sump Float Status to VTS
```

Initial Value:
```
0
```

---

### REAL Tags (16 tags)

**New Tag: REAL**

Tag Name:
```
BATT_10_504_VTS
```

Tag Comment:
```
Battery Level=0-30Vdc to VTS
```

Initial Value:
```
32.76
```

---

**New Tag: REAL**

Tag Name:
```
Dam_Level_ROC_VTS
```

Tag Comment:
```
Calculated Rate of Change (m/hr) to VTS
```

Initial Value:
```
0.0
```

---

**New Tag: REAL**

Tag Name:
```
FT_10_500_TTL_VTS
```

Tag Comment:
```
Total Flow to VTS
```

Initial Value:
```
3.75
```

---

**New Tag: REAL**

Tag Name:
```
FT_10_500_VTS
```

Tag Comment:
```
High Level Flow 0-5m3/sec to VTS
```

Initial Value:
```
1.25
```

---

**New Tag: REAL**

Tag Name:
```
FT_10_501_VTS
```

Tag Comment:
```
Low Level Flow 0-2.5m3/sec to VTS
```

Initial Value:
```
2.5
```

---

**New Tag: REAL**

Tag Name:
```
Gen_BATT_10_504_VTS
```

Tag Comment:
```
Battery Volts to VTS
```

Initial Value:
```
32.76
```

---

**New Tag: REAL**

Tag Name:
```
HCV_10_502B_VTS
```

Tag Comment:
```
Bypass Valve Position (%) to VTS
```

Initial Value:
```
50.0
```

---

**New Tag: REAL**

Tag Name:
```
HCV_10_502M_VTS
```

Tag Comment:
```
Main Valve Position (%) to VTS
```

Initial Value:
```
50.0
```

---

**New Tag: REAL**

Tag Name:
```
HCV_10_502S_VTS
```

Tag Comment:
```
Siphon Valve Position to VTS
```

Initial Value:
```
50.0
```

---

**New Tag: REAL**

Tag Name:
```
LT_10_502_Geo_VTS
```

Tag Comment:
```
Geodetic Level to VTS
```

Initial Value:
```
831.66
```

---

**New Tag: REAL**

Tag Name:
```
LT_10_502_VTS
```

Tag Comment:
```
Reservoir Level (0-100M) to VTS
```

Initial Value:
```
21.0
```

---

**New Tag: REAL**

Tag Name:
```
Main_Flow_Today_TTL_VTS
```

Tag Comment:
```
Totalized Main Flow Today to VTS
```

Initial Value:
```
9915.351
```

---

**New Tag: REAL**

Tag Name:
```
Main_Flow_Yesterday_TTL_VTS
```

Tag Comment:
```
Totalized MainFlow Yesterday to VTS
```

Initial Value:
```
101799.46
```

---

**New Tag: REAL**

Tag Name:
```
PT_10_505_VTS
```

Tag Comment:
```
High Level Press (0-6.89 kpa) to VTS
```

Initial Value:
```
172.25
```

---

**New Tag: REAL**

Tag Name:
```
PT_10_506_VTS
```

Tag Comment:
```
Low Level Pressure (0-689KPA) to VTS
```

Initial Value:
```
344.5
```

---

**New Tag: REAL**

Tag Name:
```
Siphon_Flow_Today_TTL_VTS
```

Tag Comment:
```
Siphon Flow Today (m3) to VTS
```

Initial Value:
```
19830.729
```

---

**New Tag: REAL**

Tag Name:
```
Siphon_Flow_Yesterday_TTL_VTS
```

Tag Comment:
```
Siphon Flow Yesterday (m3) to VTS
```

Initial Value:
```
203599.22
```

---

**New Tag: REAL**

Tag Name:
```
Total_Flow_Today_TTL_VTS
```

Tag Comment:
```
Totalized Total Flow Today to VTS
```

Initial Value:
```
29746.033
```

---

**New Tag: REAL**

Tag Name:
```
Total_Flow_Yesterday_TTL_VTS
```

Tag Comment:
```
Totalized Total Flow Yesterday to VTS
```

Initial Value:
```
305398.5
```

---

**New Tag: REAL**

Tag Name:
```
TT_10_503_VTS
```

Tag Comment:
```
Outside Temperature (-)40 to +30C to VTS
```

Initial Value:
```
30.0
```

---

### DINT Tags (7 tags)

**New Tag: DINT**

Tag Name:
```
Gen_CurrentRuntimer_Hrs_VTS
```

Tag Comment:
```
Generator Current Run Timer in Hrs to VTS
```

Initial Value:
```
0
```

---

**New Tag: DINT**

Tag Name:
```
Gen_CurrentRunTimer_Minutes_VTS
```

Tag Comment:
```
Generator Current Run Timer in Mins to VTS
```

Initial Value:
```
0
```

---

**New Tag: DINT**

Tag Name:
```
Gen_FailToStart_cntr_VTS
```

Tag Comment:
```
Failed to Start counts to VTS
```

Initial Value:
```
2
```

---

**New Tag: DINT**

Tag Name:
```
Gen_Runs_cntr_VTS
```

Tag Comment:
```
Generator Start Counts to VTS
```

Initial Value:
```
83
```

---

**New Tag: DINT**

Tag Name:
```
Gen_TotalRuntimer_Hrs_VTS
```

Tag Comment:
```
Generator Run Timer in Hours to VTS
```

Initial Value:
```
3
```

---

**New Tag: DINT**

Tag Name:
```
Gen_TotalRunTimer_Minutes_VTS
```

Tag Comment:
```
Generator Run Timer In Minutes to VTS
```

Initial Value:
```
0
```

---

**New Tag: DINT**

Tag Name:
```
MeterSump_Lockout_Rem_Min_VTS
```

Tag Comment:
```
Meter Sump remaining lockout time in minutes for VTS
```

Initial Value:
```
59
```

---

**New Tag: DINT**

Tag Name:
```
ValveSump_Lockout_Rem_Min_VTS
```

Tag Comment:
```
Valve Sump remaining lockout time in minutes for VTS
```

Initial Value:
```
0
```

---

## Converted Ladder Logic Rungs

### Rung 0

Rung Comment:
```
##############     Valve Control Analog     ###################
```

L5X Rung String:
```
NOP();
```

---

### Rung 1

L5X Rung String:
```
DIV(FT_10_500_SCLD,1,FT_10_500_VTS);
```

---

### Rung 2

L5X Rung String:
```
DIV(PT_10_505_SCLD,1,PT_10_505_VTS);
```

---

### Rung 3

L5X Rung String:
```
DIV(FT_10_501_SCLD,1,FT_10_501_VTS);
```

---

### Rung 4

L5X Rung String:
```
DIV(PT_10_506_SCLD,1,PT_10_506_VTS);
```

---

### Rung 5

L5X Rung String:
```
DIV(FL_10_500_TTL,1,FT_10_500_TTL_VTS);
```

---

### Rung 6

L5X Rung String:
```
MOVE(Main_Flow_Today_TTL,Main_Flow_Today_TTL_VTS);
```

---

### Rung 7

L5X Rung String:
```
MOVE(Main_Flow_Yesterday_TTL,Main_Flow_Yesterday_TTL_VTS);
```

---

### Rung 8

L5X Rung String:
```
MOVE(Siphon_Flow_Today_TTL,Siphon_Flow_Today_TTL_VTS);
```

---

### Rung 9

L5X Rung String:
```
MOVE(Siphon_Flow_Yesterday_TTL,Siphon_Flow_Yesterday_TTL_VTS);
```

---

### Rung 10

L5X Rung String:
```
MOVE(Total_Flow_Today_TTL,Total_Flow_Today_TTL_VTS);
```

---

### Rung 11

L5X Rung String:
```
MOVE(Total_Flow_Yesterday_TTL,Total_Flow_Yesterday_TTL_VTS);
```

---

### Rung 12

L5X Rung String:
```
MOVE(Bypass_Position_Percent,HCV_10_502B_VTS);
```

---

### Rung 13

L5X Rung String:
```
MOVE(Main_Position_Percent,HCV_10_502M_VTS);
```

---

### Rung 14

L5X Rung String:
```
MOVE(Siphon_Position_Percent,HCV_10_502S_VTS);
```

---

### Rung 15

Rung Comment:
```
##############     Valve Control Bool     ###################
```

L5X Rung String:
```
NOP();
```

---

### Rung 16

L5X Rung String:
```
XIC(Cal_Required_General_Alarm)OTE(Cal_Required_General_Alarm_VTS);
```

---

### Rung 17

Rung Comment:
```
############################          Generator Data to VTS          #############################
```

L5X Rung String:
```
NOP();
```

---

### Rung 18

Rung Comment:
```
Genset Running Status to VTS
```

L5X Rung String:
```
XIC(GS_10_303_RN)OTE(Genset_Running_VTS);
```

---

### Rung 19

L5X Rung String:
```
MOVE(Genset_TotalMinRun_Counter.ACC,Gen_TotalRunTimer_Minutes_VTS);
```

---

### Rung 20

L5X Rung String:
```
MOVE(Genset_TotalHrRun_Counter.ACC,Gen_TotalRuntimer_Hrs_VTS);
```

---

### Rung 21

L5X Rung String:
```
MOVE(Genset_CurrentMinRun_Counter.ACC,Gen_CurrentRunTimer_Minutes_VTS);
```

---

### Rung 22

L5X Rung String:
```
MOVE(Genset_CurrentHrRun_Counter.ACC,Gen_CurrentRuntimer_Hrs_VTS);
```

---

### Rung 23

L5X Rung String:
```
MOVE(Genset_Runs_cntr.ACC,Gen_Runs_cntr_VTS);
```

---

### Rung 24

L5X Rung String:
```
MOVE(Gen_FailToStart_cntr.ACC,Gen_FailToStart_cntr_VTS);
```

---

### Rung 25

L5X Rung String:
```
MOVE(ET_10_504_SCLD,Gen_BATT_10_504_VTS);
```

---

### Rung 26

Rung Comment:
```
Genset fault status to VTS
```

L5X Rung String:
```
XIC(GA_10_303)OTE(Genset_Fault_VTS);
```

---

### Rung 27

Rung Comment:
```
Genset low fuel alarm status to VTS
```

L5X Rung String:
```
XIC(LAL_10_303)OTE(Genset_LowFuel_VTS);
```

---

### Rung 28

Rung Comment:
```
############################          Sump Pump Data to VTS          #############################
```

L5X Rung String:
```
NOP();
```

---

### Rung 29

Rung Comment:
```
Meter Sump Remaining Lockout (Minutes) to VTS
```

L5X Rung String:
```
MOVE(MeterSump_LockoutTime,MeterSump_Lockout_Rem_Min_VTS);
```

---

### Rung 30

Rung Comment:
```
Valve Sump Remaining Lockout (Minutes) to VTS
```

L5X Rung String:
```
MOVE(ValveSump_LockoutTime,ValveSump_Lockout_Rem_Min_VTS);
```

---

### Rung 31

L5X Rung String:
```
XIC(DR_10_100_ST)OTE(RunMeterSumpPmp_VTS);
```

---

### Rung 32

L5X Rung String:
```
XIC(DR_10_101_ST)OTE(RunValveSumpPmp_VTS);
```

---

### Rung 33

L5X Rung String:
```
XIC(LAH_10_305)OTE(VTS_MeterSump_Float_VTS);
```

---

### Rung 34

L5X Rung String:
```
XIC(LAH_10_304)OTE(VTS_ValveSump_Float_VTS);
```

---

### Rung 35

Rung Comment:
```
Meter Chamber Sump Pump lockout alarm status to VTS
```

L5X Rung String:
```
XIC(MeterSump_Lockout_Alarm)OTE(MeterSump_Lockout_Alarm_VTS);
```

---

### Rung 36

Rung Comment:
```
Valve Chamber Sump Pump lockout alarm status to VTS
```

L5X Rung String:
```
XIC(ValveSump_Lockout_Alarm)OTE(ValveSump_Lockout_Alarm_VTS);
```

---

### Rung 37

Rung Comment:
```
############################          Misc Data to VTS          #############################
```

L5X Rung String:
```
NOP();
```

---

### Rung 38

Rung Comment:
```
Reservoir Level (0-100M) to VTS
```

L5X Rung String:
```
MOVE(LT_10_502_SCLD,LT_10_502_VTS);
```

---

### Rung 39

Rung Comment:
```
Geodetic Level to VTS
```

L5X Rung String:
```
MOVE(LT_10_502_SCLD_Geo,LT_10_502_Geo_VTS);
```

---

### Rung 40

Rung Comment:
```
Outside Temperature (-)40 to +30C to VTS
```

L5X Rung String:
```
MOVE(TT_10_503_SCLD,TT_10_503_VTS);
```

---

### Rung 41

Rung Comment:
```
Battery Level=0-30Vdc to VTS
```

L5X Rung String:
```
MOVE(ET_10_504_SCLD,BATT_10_504_VTS);
```

---

### Rung 42

L5X Rung String:
```
XIC(Analog_OOR_General_Alarm)OTE(Analog_OOR_General_Alarm_VTS);
```

---

### Rung 43

L5X Rung String:
```
XIC(XA_10_300)OTE(BuildingDoorSw_VTS);
```

---

### Rung 44

Rung Comment:
```
Intruder alarm status to VTS
```

L5X Rung String:
```
XIC(Intruder_Alarm)OTE(Intruder_Alarm_VTS);
```

---

### Rung 45

Rung Comment:
```
General alarm status to VTS
```

L5X Rung String:
```
XIC(General_Alarm)OTE(General_Alarm_VTS);
```

---

### Rung 46

L5X Rung String:
```
XIC(ES_10_504_STS)OTE(BatryChgOK_VTS);
```

---

### Rung 47

L5X Rung String:
```
XIC(EA_10_504_FLT)OTE(BatryChgFault_VTS);
```

---

### Rung 48

L5X Rung String:
```
XIC(XAL_10_306)OTE(BldTempLow_VTS);
```

---

### Rung 49

L5X Rung String:
```
XIC(JSH_10_308)OTE(VTS_Main_VTS);
```

---

### Rung 50

L5X Rung String:
```
XIC(VTS_Comm_Fault)OTE(VTS_Comm_Fault_VTS);
```

---

### Rung 51

L5X Rung String:
```
XIC(VTS_Comm_Fault_Latched)OTE(VTS_Comm_Fault_Latched_VTS);
```

---

### Rung 52

Rung Comment:
```
Move the calculated 5-min average Dam Level Rate of Change (m/hr) to the VTS tag.
```

L5X Rung String:
```
MOVE(Dam_Level_ROC_Avg,Dam_Level_ROC_VTS);
```

---

### Rung 53

Rung Comment:
```
Move the Dam High Level Alarm status to the VTS tag.
```

L5X Rung String:
```
XIC(Dam_Level_Alarm_HI)OTE(Dam_Level_Alarm_HI_VTS);
```

---

### Rung 54

Rung Comment:
```
Move the Dam Low Level Alarm status to the VTS tag.
```

L5X Rung String:
```
XIC(Dam_Level_Alarm_LO)OTE(Dam_Level_Alarm_LO_VTS);
```

---

### Rung 55

Rung Comment:
```
Move the Dam Rate of Change Alarm status to the VTS tag.
```

L5X Rung String:
```
XIC(Dam_Level_ROC_Alarm)OTE(Dam_Level_ROC_Alarm_VTS);
```

---

## Import Instructions

### To Import the L5X File:
1. Open Studio 5000 v36.00 or later
2. Navigate to the MainProgram in your project
3. Right-click on **Routines** → **Import Routine**
4. Select `X_Outputs_to_VTS_Routine_RLL.L5X`
5. Studio 5000 will automatically create all _VTS tags with proper data types and initial values
6. Verify the routine imported correctly

### Verification Steps:
1. Check that X_Outputs_to_VTS routine exists with 56 rungs (0-55)
2. Verify all 47 _VTS tags were created in the Controller Tags
3. Compare a few rungs visually with the Y_Outputs_To_HMI routine
4. Run a search for "_VTS" to confirm all tags are present

---

## Conversion Rules Applied

### Tag Name Conversions:
- `_HMI` suffix → `_VTS` suffix
- No suffix (legacy) → Add `_VTS` suffix

### Comment Conversions:
- "to HMI" → "to VTS"
- "for HMI display" → "for VTS display"
- "HMI" in section headers → "VTS"

### Source Tags:
- All source tags remain unchanged
- Only destination (output) tags converted

---

## Notes

- All initial values match the corresponding _HMI tags
- Source routine (Y_Outputs_To_HMI) remains unchanged
- This routine sends identical data to VTScada as HMI routine sends to C-more
- Both routines can operate simultaneously
- VTScada will need to be configured to read these _VTS tags via CIP messaging or similar protocol
