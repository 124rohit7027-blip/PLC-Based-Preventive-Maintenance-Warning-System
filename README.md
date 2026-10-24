# PLC-Based-Preventive-Maintenance-Warning-System
PLC-based preventive maintenance warning system with a counter-based maintenance interval and latched warning lamps, built in Mitsubishi GX Works3 ladder logic (FX5U PLC).
Project Scope Statement:
This project is a basic, beginner-level PLC implementation of a preventive maintenance warning system using Mitsubishi GX Works3. It demonstrates fundamental PLC concepts such as digital inputs and outputs, counter-based maintenance intervals, latched (self-holding) warning lamps, and a combined alarm output. The project is intentionally simple and is intended for educational purposes rather than as an advanced industrial automation solution.
Table of Contents
Project Overview
Project Level and Scope
Problem Statement
Project Objectives
Software and Tools Used
PLC Input/Output (I/O) Addressing Table
Working Principle
Warning Lamp Operation
Counter-Based Maintenance Interval
Ladder Logic Explanation
Self-Holding / Latching Logic
Reset Logic
Actual Ladder Diagram
Simulation Screenshot
Step-by-Step Simulation Procedure
Expected Behavior and Test Cases
Industrial Applications
Advantages
Limitations
Future Scope
Safety Precautions & Engineering Notes
Team Members
License
1. Project Overview
Unplanned machine breakdowns cause production loss, repair costs and safety risks. Preventive maintenance reduces these problems by servicing equipment at fixed intervals instead of waiting for a failure.
This project implements a PLC-based Preventive Maintenance (PM) Warning System. Using Mitsubishi MELSOFT GX Works3 and its built-in simulator, the PLC monitors machine operation signals, counts operating cycles for each machine, and switches on latched warning lamps when the maintenance interval is reached. A combined alarm output (Y4) turns on whenever any individual machine warning is active.
2. Project Level and Scope
Classification: Basic / Beginner-Level PLC Educational Project
Coursework: Programmable Logic Controllers (PLC) & SCADA
Scope Boundary:
The current scope is confined to digital logic development, input/output signal mapping, counter-based interval design, latching logic and software simulation in GX Simulator3.
Physical machine wiring, sensor selection and real power circuits were studied theoretically and are documented herein, but were not connected to live hardware during this project.
3. Problem Statement
Machines that run past their service interval wear out faster and fail without warning. Manually tracking operating hours or cycle counts for every machine is error-prone and easy to forget.
An automated control program must therefore guarantee:
Reliable counting of machine operating cycles.
A configurable maintenance interval for each machine.
A clear warning lamp that turns on when maintenance is due.
Latching of the warning, so it stays on until maintenance is acknowledged.
A combined alarm output indicating that at least one machine needs service.
A reset input to clear the warning and restart the count after maintenance.
4. Project Objectives
Design ladder logic for a preventive maintenance warning system in GX Works3.
Use PLC counters to set a maintenance interval for each machine.
Latch warning lamps using self-holding logic.
Drive a combined alarm output from the individual warning outputs.
Verify the program using GX Simulator3 before any hardware deployment.
Document the I/O mapping, logic and test results clearly.

Software and Tools Used
Tool
Purpose
Mitsubishi MELSOFT GX Works3
Ladder logic programming
GX Simulator3
Software simulation and testing
PLC model
Mitsubishi iQ-F FX5U (FX5UCPU)
Programming language
Ladder Diagram (LD)
6. PLC Input/Output (I/O) Addressing Table
Edit this table so it matches the addresses in your own GX Works3 program.
Inputs
Address
Description
X0–X3
Machine operation / cycle signals (one per machine)
X4–X7
Maintenance acknowledge / reset inputs (one per machine)
Outputs
Address
Description
Y0
Machine 1 maintenance warning lamp
Y1
Machine 2 maintenance warning lamp
Y2
Machine 3 maintenance warning lamp
Y3
Machine 4 maintenance warning lamp
Y4
Combined alarm (on when Y0, Y1 or Y2 is active)
7. Working Principle
Each machine sends a signal to a PLC input each time it completes a cycle.
A counter for that machine counts the cycles.
When the count reaches the preset maintenance interval, the counter contact closes.
The warning lamp output turns on and latches itself.
The combined alarm output Y4 turns on while any warning lamp is active.
After maintenance, the operator presses the reset input, which unlatches the lamp and resets the counter.
8. Warning Lamp Operation
The warning lamp stays off while the counter is below its preset value.
When the preset is reached, the lamp turns on and stays on, even if the machine signal stops.
The lamp turns off only when the reset input is pressed.
9. Counter-Based Maintenance Interval
Each machine has its own counter.
The counter increments on every rising edge of the machine's cycle signal.
The preset value defines the maintenance interval, for example every N cycles.
The reset input clears the counter so the next interval starts from zero.
10. Ladder Logic Explanation
The program has four main rungs plus one combined-alarm rung:
Rungs 1–4: each rung takes a machine's counter result (and its own output contact for latching) and drives one warning lamp, Y0 to Y3. A normally closed reset contact is placed in series to unlatch the lamp.
Rung 5: contacts of Y0, Y1 and Y2 are connected in parallel (OR logic) to drive the combined alarm Y4.
11. Self-Holding / Latching Logic
A normally open contact of the output is placed in parallel with the trigger condition. Once the output turns on, its own contact keeps it energised even after the trigger goes away. A normally closed reset contact in series breaks the circuit when the operator resets the system.
12. Reset Logic
The reset input is wired as a normally closed contact in the lamp rung, so pressing it breaks the latch.
The same reset also clears the counter, so counting restarts from zero.
Reset should be pressed only after the machine has actually been serviced.
13. Actual Ladder Diagram
�
Load image
The full labelled ladder diagram is also available as a PDF: ladder_diagram.pdf
14. Simulation Screenshot
�
Load image
15. Step-by-Step Simulation Procedure
Open the project in GX Works3.
Convert the program (F4) and check for errors.
Start GX Simulator3 and write the program to the simulated PLC.
Set the PLC to RUN.
Turn the machine cycle inputs ON and OFF repeatedly to count cycles.
Watch the counter value increase in the device monitor.
When the preset is reached, confirm the warning lamp output turns on and latches.
Confirm Y4 turns on.
Press the reset input and confirm the lamp, Y4 and the counter all clear.
16. Expected Behavior and Test Cases
Test
Action
Expected Result
1
Counter below preset
Warning lamp OFF
2
Counter reaches preset
Warning lamp ON and latched
3
Machine signal stops after warning
Lamp stays ON
4
Any one of Y0, Y1, Y2 ON
Combined alarm Y4 ON
5
Press reset
Lamp OFF, Y4 OFF, counter cleared
6
Two machines reach preset
Both lamps ON independently
17. Industrial Applications
Conveyor and packaging lines
CNC and machine-tool shops
Pump and compressor stations
Motor-driven production equipment
Any plant that services equipment by cycle or run count
18. Advantages
Reduces unplanned breakdowns.
Simple, low-cost logic that is easy to understand and modify.
Warnings are latched, so they cannot be missed.
Each machine is tracked independently.
Fully tested in simulation before deployment.
19. Limitations
Counts cycles only; it does not measure actual wear or condition.
Tested in simulation only, not on live hardware.
No data logging, display or remote notification.
Counter values may be lost on power failure unless latched or retentive devices are used
20. Future Scope
Use retentive counters so counts survive power loss.
Add an HMI or SCADA screen showing counts and maintenance status.
Add run-time (hour) based maintenance using timers.
Add an audible alarm and remote notification.
Log maintenance history for each machine.
21. Safety Precautions & Engineering Notes
Always isolate and lock out machines before carrying out maintenance.
Treat the warning lamp as an aid, not a replacement for scheduled inspections.
Do not press reset until maintenance has actually been completed.
Test all logic in simulation before connecting it to real equipment.
