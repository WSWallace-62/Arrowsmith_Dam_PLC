# Sump Pump Logic Overview

## System Description

The system manages two independent pumps, one for the Meter Chamber and one for the Valve Chamber. The control logic for both pumps is identical.

## Operating Modes

Each pump can be operated in two modes: "Hand" (via an HMI override) or "Auto".

- **Hand Mode**: The pump is commanded to run continuously, ignoring the level float switch.
- **Auto Mode**: The pump is controlled by its dedicated high-level float switch. To prevent rapid cycling, the pump will only start after the float has been high for a 30-second delay. Once the float drops, the pump continues to run for a 10-second stop delay emptying the pit.

## Timing Features

A minimum run timer ensures that once the pump starts, it runs for at least one second.

## Safety Features

The primary safety feature is a maximum runtime lockout. If the pump runs continuously for 30 minutes in either Hand or Auto mode, the system assumes a fault (like a stuck float or failed pump) and triggers a lockout condition. This lockout disables the pump and generates an alarm. The lockout is automatically removed after one hour, or it can be manually cleared by a global "Alarm Reset" command.

## Runtime Tracking

Finally, the logic tracks the total runtime of each pump in minutes and provides a reset function for this counter from both the HMI and VTS.
