# Analog Input Diagnostic Alarms - Out of Range Detection

## Purpose
Monitor all analog input channels (5069-IF8 module, Slot 4) for hardware diagnostic faults (Underrange and Overrange conditions) and create latched alarms for HMI display.

## Add to Routine: X_Alarms

**Note:** Uses existing `Alarm_Reset` tag (global alarm reset pushbutton)

---

## Channel 00 - Reservoir Level (LT_10_500)

### Tag Definitions

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

### Rung 25 - Ch00 Underrange Alarm

**Rung Comment:**
```
Latch alarm if Reservoir Level transmitter (Ch00) detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch00.Underrange)OTL(LT_10_500_Underrange_Latched) ,XIC(Alarm_Reset)OTU(LT_10_500_Underrange_Latched) ];
```

### Rung 26 - Ch00 Overrange Alarm

**Rung Comment:**
```
Latch alarm if Reservoir Level transmitter (Ch00) detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch00.Overrange)OTL(LT_10_500_Overrange_Latched) ,XIC(Alarm_Reset)OTU(LT_10_500_Overrange_Latched) ];
```

---

## Channel 01 - Outside Temperature (TT_10_503)

### Tag Definitions

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

### Rung 27 - Ch01 Underrange Alarm

**Rung Comment:**
```
Latch alarm if Outside Temperature transmitter (Ch01) detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch01.Underrange)OTL(TT_10_503_Underrange_Latched) ,XIC(Alarm_Reset)OTU(TT_10_503_Underrange_Latched) ];
```

### Rung 28 - Ch01 Overrange Alarm

**Rung Comment:**
```
Latch alarm if Outside Temperature transmitter (Ch01) detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch01.Overrange)OTL(TT_10_503_Overrange_Latched) ,XIC(Alarm_Reset)OTU(TT_10_503_Overrange_Latched) ];
```

---

## Channel 02 - Low Level Pressure (PT_10_501)

### Tag Definitions

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

### Rung 29 - Ch02 Underrange Alarm

**Rung Comment:**
```
Latch alarm if Low Level Pressure transmitter (Ch02) detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch02.Underrange)OTL(PT_10_501_Underrange_Latched) ,XIC(Alarm_Reset)OTU(PT_10_501_Underrange_Latched) ];
```

### Rung 30 - Ch02 Overrange Alarm

**Rung Comment:**
```
Latch alarm if Low Level Pressure transmitter (Ch02) detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch02.Overrange)OTL(PT_10_501_Overrange_Latched) ,XIC(Alarm_Reset)OTU(PT_10_501_Overrange_Latched) ];
```

---

## Channel 03 - Low Level Flow (FT_10_501)

### Tag Definitions

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

### Rung 31 - Ch03 Underrange Alarm

**Rung Comment:**
```
Latch alarm if Low Level Flow transmitter (Ch03) detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch03.Underrange)OTL(FT_10_501_Underrange_Latched) ,XIC(Alarm_Reset)OTU(FT_10_501_Underrange_Latched) ];
```

### Rung 32 - Ch03 Overrange Alarm

**Rung Comment:**
```
Latch alarm if Low Level Flow transmitter (Ch03) detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch03.Overrange)OTL(FT_10_501_Overrange_Latched) ,XIC(Alarm_Reset)OTU(FT_10_501_Overrange_Latched) ];
```

---

## Channel 04 - High Level Pressure (PT_10_502)

### Tag Definitions

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

### Rung 33 - Ch04 Underrange Alarm

**Rung Comment:**
```
Latch alarm if High Level Pressure transmitter (Ch04) detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch04.Underrange)OTL(PT_10_502_Underrange_Latched) ,XIC(Alarm_Reset)OTU(PT_10_502_Underrange_Latched) ];
```

### Rung 34 - Ch04 Overrange Alarm

**Rung Comment:**
```
Latch alarm if High Level Pressure transmitter (Ch04) detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch04.Overrange)OTL(PT_10_502_Overrange_Latched) ,XIC(Alarm_Reset)OTU(PT_10_502_Overrange_Latched) ];
```

---

## Channel 05 - High Level Flow (FT_10_500)

### Tag Definitions

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

### Rung 35 - Ch05 Underrange Alarm

**Rung Comment:**
```
Latch alarm if High Level Flow transmitter (Ch05) detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch05.Underrange)OTL(FT_10_500_Underrange_Latched) ,XIC(Alarm_Reset)OTU(FT_10_500_Underrange_Latched) ];
```

### Rung 36 - Ch05 Overrange Alarm

**Rung Comment:**
```
Latch alarm if High Level Flow transmitter (Ch05) detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch05.Overrange)OTL(FT_10_500_Overrange_Latched) ,XIC(Alarm_Reset)OTU(FT_10_500_Overrange_Latched) ];
```

---

## Channel 06 - Battery Voltage

### Tag Definitions

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

### Rung 37 - Ch06 Underrange Alarm

**Rung Comment:**
```
Latch alarm if Battery Voltage transmitter (Ch06) detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch06.Underrange)OTL(Battery_Voltage_Underrange_Latched) ,XIC(Alarm_Reset)OTU(Battery_Voltage_Underrange_Latched) ];
```

### Rung 38 - Ch06 Overrange Alarm

**Rung Comment:**
```
Latch alarm if Battery Voltage transmitter (Ch06) detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch06.Overrange)OTL(Battery_Voltage_Overrange_Latched) ,XIC(Alarm_Reset)OTU(Battery_Voltage_Overrange_Latched) ];
```

---

## Channel 07 - Spare

### Tag Definitions

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

### Rung 39 - Ch07 Underrange Alarm

**Rung Comment:**
```
Latch alarm if Spare Analog Input Ch07 detects underrange condition (signal < 3.2mA). Indicates wire break or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch07.Underrange)OTL(AI_Ch07_Underrange_Latched) ,XIC(Alarm_Reset)OTU(AI_Ch07_Underrange_Latched) ];
```

### Rung 40 - Ch07 Overrange Alarm

**Rung Comment:**
```
Latch alarm if Spare Analog Input Ch07 detects overrange condition (signal > 21mA). Indicates short circuit or sensor fault. Unlatch with global alarm reset button on separate branch.
```

**L5X Rung String:**
```
[XIC(Local:4:I.Ch07.Overrange)OTL(AI_Ch07_Overrange_Latched) ,XIC(Alarm_Reset)OTU(AI_Ch07_Overrange_Latched) ];
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

### Tags Already Available from HMI

Uses existing global `Alarm_Reset` pushbutton (no additional HMI tag mapping required)

---

## Summary

- **Total New Tags:** 16 (all BOOL latched alarm tags)
- **Total New Rungs:** 16 (Rungs 25-40 in X_Alarms routine)
- **Rung Structure:** Each rung has two parallel branches - OTL branch for latching alarm, OTU branch for reset
- **Reset Method:** Uses existing global `Alarm_Reset` pushbutton

---

## Notes

- **Underrange/Overrange thresholds are fixed by module firmware** (typically ~3.2mA and ~21mA for 4-20mA signals)
- These alarms detect **hardware/wiring faults**, not process conditions
- All alarms are **latching** and require operator acknowledgment via reset button
- **Fault conditions must clear** before alarms can be reset (OTL will re-latch if fault persists)
- Consider creating an **alarm summary screen** on HMI showing all 16 diagnostic alarms
- All analog diagnostic alarms reset with same `Alarm_Reset` button as other system alarms

---

## Testing

To test each alarm:
1. **Underrange:** Disconnect transmitter wiring at terminal block (simulates open circuit)
2. **Overrange:** Short transmitter wiring together (simulates short circuit)
3. Verify latched alarm bit sets
4. Reconnect wiring properly
5. Press global Alarm Reset button on HMI
6. Verify alarm clears

**WARNING:** Do not leave transmitters disconnected or shorted for extended periods during testing.
