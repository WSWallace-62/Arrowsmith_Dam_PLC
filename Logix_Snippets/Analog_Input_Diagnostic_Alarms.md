# Analog Input Diagnostic Alarms - Out of Range Detection

## Purpose
Monitor all analog input channels (5069-IF8 module, Slot 4) for hardware diagnostic faults (Underrange and Overrange conditions) and create latched alarms for HMI display.

## Add to Routine: X_Alarms

---

## New Tag Definitions

### Channel 00 - Reservoir Level (LT_10_500)

**Tag Name:** `LT_10_500_Underrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - Reservoir Level transmitter signal below 3.2mA (wire break or sensor fault)
```

**Tag Name:** `LT_10_500_Overrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - Reservoir Level transmitter signal above 21mA (short circuit or sensor fault)
```

### Channel 01 - Outside Temperature (TT_10_503)

**Tag Name:** `TT_10_503_Underrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - Outside Temperature transmitter signal below 3.2mA (wire break or sensor fault)
```

**Tag Name:** `TT_10_503_Overrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - Outside Temperature transmitter signal above 21mA (short circuit or sensor fault)
```

### Channel 02 - Low Level Pressure (PT_10_501)

**Tag Name:** `PT_10_501_Underrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - Low Level Pressure transmitter signal below 3.2mA (wire break or sensor fault)
```

**Tag Name:** `PT_10_501_Overrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - Low Level Pressure transmitter signal above 21mA (short circuit or sensor fault)
```

### Channel 03 - Low Level Flow (FT_10_501)

**Tag Name:** `FT_10_501_Underrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - Low Level Flow transmitter signal below 3.2mA (wire break or sensor fault)
```

**Tag Name:** `FT_10_501_Overrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - Low Level Flow transmitter signal above 21mA (short circuit or sensor fault)
```

### Channel 04 - High Level Pressure (PT_10_502)

**Tag Name:** `PT_10_502_Underrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - High Level Pressure transmitter signal below 3.2mA (wire break or sensor fault)
```

**Tag Name:** `PT_10_502_Overrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - High Level Pressure transmitter signal above 21mA (short circuit or sensor fault)
```

### Channel 05 - High Level Flow (FT_10_500)

**Tag Name:** `FT_10_500_Underrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - High Level Flow transmitter signal below 3.2mA (wire break or sensor fault)
```

**Tag Name:** `FT_10_500_Overrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - High Level Flow transmitter signal above 21mA (short circuit or sensor fault)
```

### Channel 06 - Battery Voltage

**Tag Name:** `Battery_Voltage_Underrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - Battery Voltage transmitter signal below 3.2mA (wire break or sensor fault)
```

**Tag Name:** `Battery_Voltage_Overrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - Battery Voltage transmitter signal above 21mA (short circuit or sensor fault)
```

### Channel 07 - Spare

**Tag Name:** `AI_Ch07_Underrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - Analog Input Ch07 signal below 3.2mA (wire break or sensor fault)
```

**Tag Name:** `AI_Ch07_Overrange_Latched`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
Latched alarm - Analog Input Ch07 signal above 21mA (short circuit or sensor fault)
```

### Master Alarm Reset Tag

**Tag Name:** `AI_Diagnostic_Alarms_Reset`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
HMI pushbutton - Reset all analog input diagnostic alarms (underrange/overrange)
```

---

## Ladder Logic Rungs

### Rung - Channel 00 Underrange Alarm

**Rung Comment:**
```
Latch alarm if Reservoir Level transmitter (Ch00) detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch00.Underrange)OTL(LT_10_500_Underrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(LT_10_500_Underrange_Latched);
```

---

### Rung - Channel 00 Overrange Alarm

**Rung Comment:**
```
Latch alarm if Reservoir Level transmitter (Ch00) detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch00.Overrange)OTL(LT_10_500_Overrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(LT_10_500_Overrange_Latched);
```

---

### Rung - Channel 01 Underrange Alarm

**Rung Comment:**
```
Latch alarm if Outside Temperature transmitter (Ch01) detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch01.Underrange)OTL(TT_10_503_Underrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(TT_10_503_Underrange_Latched);
```

---

### Rung - Channel 01 Overrange Alarm

**Rung Comment:**
```
Latch alarm if Outside Temperature transmitter (Ch01) detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch01.Overrange)OTL(TT_10_503_Overrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(TT_10_503_Overrange_Latched);
```

---

### Rung - Channel 02 Underrange Alarm

**Rung Comment:**
```
Latch alarm if Low Level Pressure transmitter (Ch02) detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch02.Underrange)OTL(PT_10_501_Underrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(PT_10_501_Underrange_Latched);
```

---

### Rung - Channel 02 Overrange Alarm

**Rung Comment:**
```
Latch alarm if Low Level Pressure transmitter (Ch02) detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch02.Overrange)OTL(PT_10_501_Overrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(PT_10_501_Overrange_Latched);
```

---

### Rung - Channel 03 Underrange Alarm

**Rung Comment:**
```
Latch alarm if Low Level Flow transmitter (Ch03) detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch03.Underrange)OTL(FT_10_501_Underrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(FT_10_501_Underrange_Latched);
```

---

### Rung - Channel 03 Overrange Alarm

**Rung Comment:**
```
Latch alarm if Low Level Flow transmitter (Ch03) detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch03.Overrange)OTL(FT_10_501_Overrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(FT_10_501_Overrange_Latched);
```

---

### Rung - Channel 04 Underrange Alarm

**Rung Comment:**
```
Latch alarm if High Level Pressure transmitter (Ch04) detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch04.Underrange)OTL(PT_10_502_Underrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(PT_10_502_Underrange_Latched);
```

---

### Rung - Channel 04 Overrange Alarm

**Rung Comment:**
```
Latch alarm if High Level Pressure transmitter (Ch04) detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch04.Overrange)OTL(PT_10_502_Overrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(PT_10_502_Overrange_Latched);
```

---

### Rung - Channel 05 Underrange Alarm

**Rung Comment:**
```
Latch alarm if High Level Flow transmitter (Ch05) detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch05.Underrange)OTL(FT_10_500_Underrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(FT_10_500_Underrange_Latched);
```

---

### Rung - Channel 05 Overrange Alarm

**Rung Comment:**
```
Latch alarm if High Level Flow transmitter (Ch05) detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch05.Overrange)OTL(FT_10_500_Overrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(FT_10_500_Overrange_Latched);
```

---

### Rung - Channel 06 Underrange Alarm

**Rung Comment:**
```
Latch alarm if Battery Voltage transmitter (Ch06) detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch06.Underrange)OTL(Battery_Voltage_Underrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(Battery_Voltage_Underrange_Latched);
```

---

### Rung - Channel 06 Overrange Alarm

**Rung Comment:**
```
Latch alarm if Battery Voltage transmitter (Ch06) detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch06.Overrange)OTL(Battery_Voltage_Overrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(Battery_Voltage_Overrange_Latched);
```

---

### Rung - Channel 07 Underrange Alarm (Spare)

**Rung Comment:**
```
Latch alarm if Spare Analog Input Ch07 detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch07.Underrange)OTL(AI_Ch07_Underrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(AI_Ch07_Underrange_Latched);
```

---

### Rung - Channel 07 Overrange Alarm (Spare)

**Rung Comment:**
```
Latch alarm if Spare Analog Input Ch07 detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with master reset button.
```

**L5X Rung String:**
```
XIC(Local:4:I.Ch07.Overrange)OTL(AI_Ch07_Overrange_Latched)XIC(AI_Diagnostic_Alarms_Reset)OTU(AI_Ch07_Overrange_Latched);
```

---

### Rung - One-Shot for Reset Button

**Rung Comment:**
```
One-shot the reset command to prevent multiple scan cycles from interfering with alarm reset logic. Resets all analog input diagnostic alarms when operator presses HMI reset button.
```

**L5X Rung String:**
```
ONS(AI_Diag_Reset_ONS)OTU(AI_Diagnostic_Alarms_Reset);
```

**Additional Tag Required:**

**Tag Name:** `AI_Diag_Reset_ONS`  
**Data Type:** `BOOL`  
**Tag Comment:**
```
One-shot bit for analog input diagnostic alarm reset button
```

---

## HMI Integration

### Tags to Send to HMI (Add to Y_Outputs_To_HMI routine)

All 16 latched alarm tags should be mapped to HMI for display and alarming:
- `LT_10_500_Underrange_Latched`
- `LT_10_500_Overrange_Latched`
- `TT_10_503_Underrange_Latched`
- `TT_10_503_Overrange_Latched`
- `PT_10_501_Underrange_Latched`
- `PT_10_501_Overrange_Latched`
- `FT_10_501_Underrange_Latched`
- `FT_10_501_Overrange_Latched`
- `PT_10_502_Underrange_Latched`
- `PT_10_502_Overrange_Latched`
- `FT_10_500_Underrange_Latched`
- `FT_10_500_Overrange_Latched`
- `Battery_Voltage_Underrange_Latched`
- `Battery_Voltage_Overrange_Latched`
- `AI_Ch07_Underrange_Latched`
- `AI_Ch07_Overrange_Latched`

### Tags to Receive from HMI (Add to C_Inputs_from_HMI routine)

Map the reset button:
- `AI_Diagnostic_Alarms_Reset` (momentary pushbutton on HMI alarm screen)

---

## Notes

- **Underrange/Overrange thresholds are fixed by module firmware** (typically ~3.2mA and ~21mA for 4-20mA signals)
- These alarms detect **hardware/wiring faults**, not process conditions
- All alarms are **latching** and require operator acknowledgment via reset button
- Reset button uses one-shot to ensure clean reset behavior
- **Fault conditions must clear** before alarms can be reset (can't reset while fault persists)
- Consider creating an **alarm summary screen** on HMI showing all 16 diagnostic alarms

---

## Testing

To test each alarm:
1. **Underrange:** Disconnect transmitter wiring at terminal block (simulates open circuit)
2. **Overrange:** Short transmitter wiring together (simulates short circuit)
3. Verify latched alarm bit sets
4. Reconnect wiring properly
5. Press HMI reset button
6. Verify alarm clears

**WARNING:** Do not leave transmitters disconnected or shorted for extended periods during testing.
