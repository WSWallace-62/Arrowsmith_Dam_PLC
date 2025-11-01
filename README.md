# Arrowsmith Dam Control System

This repository contains the Rockwell Studio 5000 PLC logic for the Arrowsmith Dam, controlling the main, bypass, and siphon valves.

**PLC Hardware:** CompactLogix 5069-L306ER
**Studio 5000 Version:** v36.00

---

## Project Overview

This program controls the three primary valves at the dam. It includes all interlocking, HMI/SCADA command handling, and simulated positioning. The logic is designed to be imported into the main controller project.

---

## Key Files

* **`Arrowsmith_Dam.L5X`**: The main controller project file.
* **`Valves_Routine_RLL.L5X`**: A standalone export of the "Valves" routine. Contains all logic for the Bypass, Main, and Siphon valves.

---

## I/O Configuration

* **Controller:** 5069-L306ER
* **Slot 1:** 5069-IB16 (Digital Inputs)
* **Slot 2:** 5069-IB16 (Digital Inputs)
* **Slot 3:** 5069-OB16 (Digital Outputs)
* **Slot 4:** 5069-IF8 (Analog Inputs)

---

## How to Use

To use this logic:

1.  Open the main `Arrowsmith_Dam.L5X` project.
2.  Alternatively, you can import the `Valves_Routine_RLL_R1.L5X` file by right-clicking on the "MainProgram" and selecting "Import Routine...".

---

## Change Log

* **Oct 24, 2025:** Added "Cooldown" and "Anti-Repeat" (Cmd_Held) logic to all three valves.
* **Oct 23, 2025:** Initial commit of valve logic.
* **Nov 01, 2025:** Replaced old Routine L5X files with current Project L5X file.
