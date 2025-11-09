# Arrowsmith Dam Control System

This repository contains the Rockwell Studio 5000 PLC logic for the Arrowsmith Dam (Parksville), controlling the main, bypass, and siphon valves, along with monitoring systems, sump pump control, and auxiliary equipment.

**PLC Hardware:** CompactLogix 5069-L306ER  
**Studio 5000 Version:** v36.00  
**Project Name:** Arrowsmith_Dam  
**Communication:** Ethernet 192.168.0.130

---

## Project Overview

This comprehensive control system manages three motorized dam valves with sophisticated movement control, position simulation, and safety interlocks. The system accepts commands from three sources: local panel switches, HMI (C-more panel), and VTScada remote SCADA system. All valve operations include intelligent cooldown periods and anti-repeat logic to protect actuators from damage.

### Core Features
- **Triple-source valve control** (Panel, HMI, VTScada)
- **Simulated valve positioning** with 100ms pulse-based position tracking
- **Adjustable timed movements** with operator-configurable presets
- **Cooldown protection** preventing rapid valve cycling
- **Anti-repeat interlocks** requiring command release before new operations
- **VTScada communication watchdog** with 10-second heartbeat monitoring
- **Automatic sump pump control** with runtime protection and lockout
- **Building entry alarm system** with auto-arm and entry delay features
- **Analog input scaling** for sensors (levels, flows, temperatures, pressures)

---

## Repository Structure

```
Arrowsmith_Dam_PLC/
├── README.md
├── AI_INSTRUCTIONS.md
├── Valve_Calibration_AOI_Implementation_Summary.md
├── L5X_Files/
│   ├── Arrowsmith_Dam_Nov9_R2.L5X        # Complete project file (current)
│   ├── Valve_Position_Calibration_R2_AOI.L5X  # Valve calibration AOI
│   └── _PowerUp_Init_Routine_RLL.L5X     # Power-up initialization routine
├── Tag_Info/
│   ├── Arrowsmith_Dam_Nov9_R2_Controller_Tags.CSV
│   └── C-more-TagDataBase_Nov9_R0.CSV
├── Misc_Docs/
│   └── IOChecksheet-SFAT_Arrowsmith - Update Oct 28 by Burke.csv
└── HMI_Images/
```

---

## I/O Configuration

### Hardware Layout
* **Controller:** 5069-L306ER (Slot 0)
* **Slot 1:** 5069-IB16/A - Digital Inputs (16 points)
* **Slot 2:** 5069-IB16/A - Digital Inputs (16 points)  
* **Slot 3:** 5069-OB16/B - Digital Outputs with Diagnostics (16 points)
* **Slot 4:** 5069-IF8/B - Analog Inputs (8 channels, 4-20mA)

### Digital Input Assignments

**IB16_1 (Slot 1):**
- Genset status (Running, Fault, Low Fuel)
- Sump level monitors (Valve Chamber, Meter Chamber)
- UPS/Battery status (Charge OK, Charge Fault)
- Building monitoring (Door Switch, Temperature Low)
- AC Power status

**IB16_2 (Slot 2):**
- Local operator panel switches (6 switches: Open/Close for 3 valves)
- Valve position limit switches:
  - Bypass: ZSO_10_302 (Fully Open), ZSC_10_302 (Fully Closed)
  - Main: ZSO_10_300 (Fully Open), ZSC_10_300 (Fully Closed)
  - Siphon: ZSC_10_301 (Fully Closed)

### Digital Output Assignments (Slot 3)
- **PT00-01:** Bypass Valve (Open/Close)
- **PT02-03:** Main Valve (Open/Close)
- **PT04-05:** Siphon Valve (Open/Close)
- **PT06:** Meter Chamber Sump Pump
- **PT07:** Valve Chamber Sump Pump
- **PT09:** Video Power
- **PT14:** Starlink Power
- **PT15:** Station Active Indicator

### Analog Input Assignments (Slot 4)
All channels configured for 4-20mA, scaled 0-100%:
- **CH00:** Reservoir Level (LT_10_500)
- **CH01:** Outside Temperature (TT_10_503), -40°C to +30°C
- **CH02:** Low Level Pressure (PT_10_501)
- **CH03:** Low Level Flow (FT_10_501)
- **CH04:** High Level Pressure (PT_10_502)
- **CH05:** High Level Flow (FT_10_500), 0-5 m³/sec
- **CH06:** Battery Voltage (0-30 VDC)
- **CH07:** Spare

---

## Program Structure

The MainProgram executes the following routines in order:

1. **A_Input_Mapping** - Maps physical I/O to internal tags
2. **B_Inputs_from_VTS** - Processes VTScada commands and watchdog
3. **C_Inputs_from_HMI** - Processes HMI operator commands
4. **Genset** - Generator monitoring and control logic
5. **MainRoutine** - Calls subordinate routines in sequence
6. **Misc** - Utility functions and miscellaneous control
7. **SumpPumps** - Automatic sump pump control with protection
8. **Valve_Bypass** - Bypass valve control logic
9. **Valve_Main** - Main valve control logic
10. **Valve_Siphon** - Siphon valve control logic
11. **Valve_Pos_Cal** - Semi-automatic valve position calibration using AOI
12. **X_Alarms** - Alarm processing and latching
13. **X_Outputs_to_VTS** - Sends status to VTScada
14. **Y_Outputs_To_HMI** - Sends status and data to HMI
15. **Z_Output_Mapping** - Maps internal tags to physical outputs

### Special Programs
- **_PowerUp_Init** - Executes once on controller power-up to initialize critical tags

---

## Valve Control Logic Details

### Operating Modes
Each valve operates in one of two modes:
- **Local Mode:** Panel switches (HSO/HSC) have control
- **Remote Mode:** HMI or VTScada commands control the valve (when panel switches are neutral)

### Valve Movement Sequence

1. **Command Initiation**
   - Operator issues command via Panel Switch, HMI, or VTScada
   - System validates: Remote Mode permissive, Not in cooldown, Commands not held

2. **Movement Timer Selection**
   - HMI or VTScada provides movement duration setpoint (seconds)
   - Timer preset automatically selected based on command source
   - Preset converted to milliseconds for internal timer

3. **Valve Operation**
   - Appropriate permissive bit latches (Open_Permissive or Close_Permissive)
   - Output energizes (if not already at limit switch)
   - Movement timer starts
   - Position counter updates every 100ms

4. **Position Tracking**
   - 100ms free-running pulse timer (T_Position_Pulse)
   - Position increment calculated: `(Max_Position / Full_Stroke_Time) × 0.1`
   - Counter increments (opening) or decrements (closing)
   - Limit switches reset position to known values

5. **Movement Termination**
   - Stops when: Movement timer done **OR** Limit switch activated
   - Permissive and remote command bits unlatch
   - Cooldown period begins

6. **Cooldown Period (Anti-Cycling Protection)**
   - Duration equals the valve movement time that just completed
   - `Cooldown_Active` bit prevents new commands
   - `Cmd_Held` interlock latches

7. **Anti-Repeat Release**
   - All command sources (Panel, HMI, VTScada) must return to neutral
   - `Cmd_Held` bit unlatches
   - System ready for next command

### Valve-Specific Parameters

| Valve   | Full Stroke Time | Position Range | Timer Preset Tag          |
|---------|------------------|----------------|---------------------------|
| Bypass  | 100 seconds      | 0 - 5,000,000  | Actual_Bypass_MoveTmr_pre |
| Main    | Variable         | 0 - 5,000,000  | Actual_Main_MoveTmr_pre   |
| Siphon  | Variable         | 0 - 5,000,000  | Actual_Siphon_MoveTmr_pre |

---

## Sump Pump Control

### Features
- **Automatic level-based start/stop**
- **Manual override capability**
- **30-minute maximum runtime protection**
- **1-hour lockout after max runtime exceeded**
- **Minimum run time enforcement**
- **Runtime accumulation and display**

### Protection Logic
- If pump runs continuously for 30 minutes → Lockout activated
- During lockout: Pump disabled for 60 minutes
- Lockout countdown displayed to operators
- Lockout resettable by operator after timeout

---

## Communication & Monitoring

### VTScada Watchdog
- **Heartbeat Tag:** `VTS_Watchdog_In` (must be continuously incremented by SCADA)
- **Watchdog Timer:** 10 seconds
- **Fault Condition:** Timer done = Communication lost
- **Fault Action:** Sets `VTS_Comm_Fault` and latches `VTS_Comm_Fault_Latched`
- **Recovery:** Operator can reset latched fault after communication restored

### Data Exchange
**To VTScada/HMI:**
- Valve positions (percentage)
- Limit switch states
- Alarm statuses
- Flow totalization (daily/yesterday)
- Analog sensor values
- Sump pump status and runtime

**From VTScada/HMI:**
- Valve movement commands
- Movement timer presets
- Manual overrides
- Alarm resets

---

## Valve Position Calibration

### Valve_Position_Calibration Add-On Instruction (AOI)

A reusable AOI provides **semi-automatic valve position calibration** for all three motorized valves. The calibration sequence establishes accurate position tracking by setting raw minimum/maximum values and measuring full stroke time.

**Key Features:**
- **3 user confirmations** (initial start, closed position, open position)
- **Automatic 50% verification** after calibration
- **Safety timeouts** (60 seconds for movements, 10 seconds for confirmations)
- **Abort capability** at any step
- **Stroke time measurement** in seconds
- **Delay compensation** (500ms adjustable) for valve stickage

**Implementation:**
- **Hybrid control approach:** Single set of `CAL_...` control tags
- **Valve selector:** `CAL_Valve_Select` (0=None, 1=Bypass, 2=Main, 3=Siphon)
- **One HMI screen** works for all three valves
- **Prevents simultaneous calibrations**

For detailed documentation, see `Valve_Calibration_AOI_Implementation_Summary.md`.

---

## Add-On Instructions (AOI)

### Valve_Position_Calibration (Rev 2.0)
Semi-automatic valve calibration AOI for establishing position tracking parameters.

**Parameters:**
- **Inputs:** `Enable`, `Confirm_CMD`, `Abort_CMD`, `ZSC`, `ZSO`, timeout/delay presets
- **Outputs:** `Valve_Open_CMD`, `Valve_Close_CMD`, status flags, `Cal_Step`
- **InOuts:** `Position_Counter`, `InRawMin`, `InRawMax`, `Full_Stroke_Time_SP`, `Position_Percent`

**Calibration States:** 0→5→10→15→20→25→30→31→35→40→50 (plus abort/fault states 98/99)

### Analog_Scaling (Rev 1.0)
Custom AOI for linear scaling of analog inputs.

**Parameters:**
- `RAW_DATA` (Input) - Raw analog value (typically 0-100%)
- `cfgRAW_MIN` / `cfgRAW_MAX` - Raw value range
- `cfgSCALED_MIN` / `cfgSCALED_MAX` - Engineering unit range
- `SCALED_DATA` (Output) - Scaled result

**Example Usage:**
```
TT_10_503 (Outside Temperature)
  RAW: 0-100% → SCALED: -40°C to +30°C
  
FT_10_500 (High Level Flow)
  RAW: 0-100% → SCALED: 0-5 m³/sec
```

---

## Safety Features

1. **Valve Cooldown Protection** - Prevents rapid cycling and actuator damage
2. **Anti-Repeat Interlocks** - Requires deliberate command release
3. **Sump Pump Runtime Limits** - Protects against continuous operation failures
4. **Communication Watchdog** - Detects SCADA connection loss
5. **Limit Switch Monitoring** - Prevents overtravel
6. **Building Entry Alarm** - Security monitoring with auto-arm feature

---

## How to Use This Project

### Opening the Project
1. Launch Rockwell Studio 5000 v36.00 or later
2. Open `L5X_Files/Arrowsmith_Dam_Nov9_R2.L5X`
3. Controller will appear offline until connected to hardware

### Downloading to Controller
1. Verify Ethernet connection to 192.168.0.130
2. Right-click controller → Download
3. Select "Download" and follow prompts
4. Run Mode: Switch to "Remote Run"

### Importing Individual Routines
- Right-click "MainProgram" → Import Routine
- Select desired routine from `L5X_Files/`
- Useful for updates without full project download

### Commissioning Notes
- **Valve Full Stroke Times:** Adjust `xxx_Full_Stroke_Time_SP` tags during commissioning
- **Position Calibration:** Exercise each valve fully open/closed to verify limit switches
- **Analog Scaling:** Verify all AOI parameters match field instrumentation
- **Watchdog Setup:** Configure VTScada to increment `VTS_Watchdog_In` every scan

---

## Change Log

* **Nov 09, 2025 (R2):** Split Valves routine into three separate routines (Valve_Bypass, Valve_Main, Valve_Siphon) for improved organization and maintainability
* **Nov 08, 2025:** Added Valve_Position_Calibration AOI with hybrid control approach
* **Nov 05, 2025:** Updated README to reflect current project state
* **Nov 01, 2025:** Replaced old Routine L5X files with current Project L5X file
* **Oct 24, 2025:** Added "Cooldown" and "Anti-Repeat" (Cmd_Held) logic to all three valves
* **Oct 23, 2025:** Initial commit of valve logic
* **Sep 17, 2025:** Project created

---

## Notes

- Position feedback is **simulated** using time-based calculations (no analog position transmitters installed)
- System designed for integration with C-more HMI and VTScada SCADA system
- All timers use millisecond timebase; operator setpoints converted from seconds
- Building alarm system includes 30-minute auto-arm and 45-second entry delay
