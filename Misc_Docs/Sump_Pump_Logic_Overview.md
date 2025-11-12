# Sump Pump Logic Overview

## System Description

The system manages two independent pumps, one for the Meter Chamber and one for the Valve Chamber. The control logic for both pumps is identical.

## Operating Modes

Each pump can be operated in three modes: "Hand", "Auto", or "Off".

- **Hand Mode**: The pump is commanded to run continuously, ignoring the level float switch.
- **Auto Mode**: The pump is controlled by its dedicated high-level float switch. To prevent rapid cycling, the pump will only start after the float has been high for a 30-second delay. Once the float drops, the pump continues to run for a 10-second stop delay emptying the pit.
- **Off Mode**: The pump is turned off. This mode can also be used to manually reset a lockout condition if one is active.

## Timing Features

A minimum run timer ensures that once the pump starts, it runs for at least one second.  This value can be adjusted by the Commissioning Eng.

## Safety Features

The primary safety feature is a maximum runtime lockout. If the pump runs continuously for 30 minutes in either Hand or Auto mode, the system assumes a fault (like a stuck float or failed pump) and triggers a lockout condition. 

When a lockout occurs:
- The pump is immediately disabled and cannot run
- An alarm is generated to alert operators
- The system automatically switches to "Off" mode

The lockout can be cleared in three ways:
1. Automatically after one hour
2. By pressing the global "Alarm Reset" button
3. By manually selecting "Off" mode on the operator screen

## Runtime Tracking

Finally, the logic tracks the total runtime of each pump in minutes and provides a reset function for this counter from both the HMI and VTS.
