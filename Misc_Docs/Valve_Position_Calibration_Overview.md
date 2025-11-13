# Valve Position Calibration Overview

## System Description

The Valve Position Calibration system is a semi-automatic Add-On Instruction (AOI) that performs a complete calibration of a valve's position feedback system. The calibration establishes accurate position scaling by measuring the actual stroke time and setting the minimum and maximum raw position values.

Each valve uses this calibration routine independently to ensure its simulated position accurately reflects the physical valve position throughout its full range of travel.

## Calibration Purpose

The calibration routine serves three primary purposes:

1. **Measure Full Stroke Time** - Determines the actual time required for the valve to travel from fully closed to fully open, which is used to calculate position changes during normal operation.

2. **Set Position Scaling** - Establishes the minimum (0) and maximum (5,000,000) raw position counter values that correspond to the fully closed and fully open limit switches.

3. **Verify Mid-Position Accuracy** - Confirms that the position simulation can accurately position the valve at 50% by commanding movement to the midpoint and verifying the result.

## Calibration Sequence

The calibration follows a multi-step sequence with built-in user confirmations at critical points:

### Step 0: Idle State
The routine waits for the operator to press the Enable button on the HMI. When enabled, the system advances to the confirmation state and starts a 10-second confirmation window.

### Step 5: Awaiting Initial Confirmation
The operator has 10 seconds to press the Confirm button to proceed with calibration. If no confirmation is received within this window, the routine returns to idle. Once confirmed, calibration begins.

### Step 10: Closing to Zero Reference
The routine commands the valve to close by activating the Close output. The valve continues closing until the fully closed limit switch (ZSC) activates or a 60-second timeout expires. This establishes the zero position reference point.

### Step 15: Closed Position Confirmed
When the closed limit is reached, the system sets both the minimum raw position value (InRawMin) and the position counter to zero. The routine then waits for operator confirmation before proceeding to the opening phase.

### Step 20: Delay Before Opening
A 500-millisecond delay is applied while the Open command is active. This delay compensates for any initial valve stickage and ensures the stroke timer starts measuring only after the valve begins moving smoothly.

### Step 25: Opening and Measuring Stroke Time
The routine commands the valve to open and simultaneously starts the stroke timer. The valve continues opening until the fully open limit switch (ZSO) activates or a 60-second timeout expires. The stroke timer accumulates the actual travel time in milliseconds.

### Step 30: Open Position Confirmed
When the open limit is reached, the system performs several actions:
- Sets the maximum raw position value (InRawMax) to 5,000,000
- Sets the position counter to 5,000,000
- Converts the measured stroke time from milliseconds to seconds
- Stores the stroke time in the Full Stroke Time setpoint

The routine then waits for operator confirmation to proceed to the verification step.

### Step 31: Waiting for Mid-Position Confirmation
The operator reviews the measured stroke time and confirms to proceed with the 50% position verification test.

### Step 35: Moving to 50% Position
The routine commands the valve to move to the 50% position (2,500,000 raw counts). The system actively controls the valve:
- If position is below 2,450,000 (less than 49%), the Open command is activated
- If position is above 2,550,000 (greater than 51%), the Close command is activated
- When position reaches the target range (2,450,000 to 2,550,000), both commands are deactivated

This active positioning verifies that the calibration values allow accurate mid-range positioning with a tolerance of ±50,000 counts (±1%).

### Step 40: Calibration Complete
The calibration has successfully completed. The Cal_Complete flag is set momentarily before advancing to cleanup.

### Step 50: Cleanup
All calibration flags are reset, and the routine returns to the idle state, ready for the next calibration cycle.

## Abort and Fault Handling

### User Abort (Step 98)
The operator can abort the calibration at any time during active steps by pressing the Abort button. When aborted:
- All valve commands are deactivated
- The Cal_Aborted flag is set
- The routine waits for the Enable button to be released before returning to idle

### Timeout Fault (Step 99)
If the valve fails to reach the closed limit (Step 10) or open limit (Step 25) within 60 seconds, a timeout fault occurs:
- All valve commands are deactivated
- The Cal_Failed flag is set
- The routine waits for the Enable button to be cycled before returning to idle

## Key Parameters

### Input Parameters
- **Enable** - HMI button to start calibration
- **Confirm_CMD** - HMI button for operator confirmations at key steps
- **Abort_CMD** - HMI button to abort calibration at any time
- **ZSC** - Fully closed limit switch input
- **ZSO** - Fully open limit switch input
- **Timeout_Preset** - Movement timeout in milliseconds (default: 60,000 = 1 minute)
- **Delay_Preset** - Start delay before opening in milliseconds (default: 500)
- **Confirmation_Preset** - Confirmation timeout in milliseconds (default: 10,000 = 10 seconds)

### Output Parameters
- **Valve_Open_CMD** - Command to open the valve
- **Valve_Close_CMD** - Command to close the valve
- **Cal_In_Progress** - Indicates calibration is actively running
- **Cal_Complete** - Indicates successful calibration completion
- **Cal_Aborted** - Indicates operator aborted the calibration
- **Cal_Failed** - Indicates calibration failed due to timeout
- **Awaiting_Confirm** - Indicates waiting for operator confirmation
- **Cal_Step** - Current step number for display and troubleshooting

### InOut Parameters (Modified by Calibration)
- **Position_Counter** - Real-time position counter (updated during calibration)
- **InRawMin** - Minimum raw position value (set to 0 at closed limit)
- **InRawMax** - Maximum raw position value (set to 5,000,000 at open limit)
- **Full_Stroke_Time_SP** - Measured full stroke time in seconds (calculated from actual travel)
- **Position_Percent** - Position percentage for display (calculated from position counter)

## Integration with Normal Valve Control

During calibration, the AOI outputs (Valve_Open_CMD and Valve_Close_CMD) must be connected to override the normal valve control logic. The Cal_In_Progress flag can be used to block normal operator commands and ensure the calibration routine has exclusive control of the valve.

Once calibration is complete, the measured Full_Stroke_Time_SP value is used by the normal position simulation logic to accurately calculate position changes based on the time the Open or Close outputs are active.

## Confirmation Strategy

The calibration requires operator confirmation at three critical points:

1. **Initial Start** - Confirms the operator is ready to begin the automated sequence
2. **After Closing** - Allows operator to verify the valve reached the closed limit before opening
3. **After Opening** - Allows operator to review the measured stroke time before proceeding to the 50% test

These confirmations ensure the operator maintains awareness and control throughout the calibration process while still automating the time-critical measurement steps.
