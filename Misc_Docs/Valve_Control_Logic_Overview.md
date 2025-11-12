# Valve Control Logic Overview

## System Description

The system controls three valves (Main, Bypass, and Siphon) which can be controlled from three sources: local hardwired panel switches, the HMI, or the remote VTS (SCADA) system.

Each valve operates independently with its own dedicated control logic, ensuring that commands and movements for one valve do not interfere with the others.

## Command Sources and Acceptance

A command to Open or Close a valve can originate from the local panel switches, the HMI, or VTS. 

The system determines whether to accept commands from the HMI or VTS based on the position of the local panel selector switches (HSO for Open and HSC for Close). When both selector switches are in their neutral (off) position, the valve is in "Remote" mode and will accept commands from the HMI or VTS. If either selector switch is held in the Open or Close position, the valve responds only to that local panel command and ignores remote commands.

Any new command is accepted only if the valve is not already moving or in its post-move "cooldown" period.

## Travel Time Selection

The system uses a "travel time," specified in seconds, to determine how long the physical 'Open' or 'Close' output is energized. This travel time is selected based on the command's source:

- Commands originating from the HMI or the local panel switches will use the travel time setpoint defined on the HMI.
- Commands originating from VTS will use the separate travel time setpoint defined in VTS.

## Cooldown Feature

A cooldown feature is included to prevent excessive valve cycling. After any move completes, the valve enters a cooldown state where no new commands are accepted. This cooldown period helps protect the valve actuator from wear.

The cooldown timer must expire AND all operator commands (from HMI, VTS, and local switches) must be released before a new valve movement can begin. This prevents operators from rapidly cycling the valve and ensures any active command is fully released before accepting a new one.

## Position Simulation

The valve positions are simulated since there are no physical position sensors installed. This virtual position is calculated by moving a raw position counter up or down whenever the 'Open' or 'Close' outputs are active. 

The rate at which this counter moves is determined by the "Full Stroke Time" setpoint. Each valve has its own Full Stroke Time value, which represents the total number of seconds required for that specific valve to travel from fully closed to fully open (or vice versa).

The system uses this Full Stroke Time to calculate how much the position should change every tenth of a second. For example, if a valve has a Full Stroke Time of 60 seconds, the position counter advances approximately 1.67% every tenth of a second while the valve is moving.

This "Full Stroke Time" setpoint is currently a default value and will eventually be measured and set automatically by the Position Calibrate routine (which is not yet functional) to ensure the simulation accurately matches the actual valve travel.
