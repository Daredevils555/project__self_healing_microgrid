Design and Implementation of a Self-Healing DC Microgrid using ESP32 and CAN Communication

⸻

  Objective

The objective of this project is to design a small-scale DC microgrid capable of:
	•	Monitoring dual power sources
	•	Monitoring load current and voltage
	•	Detecting electrical faults (overcurrent, undervoltage, abnormal load behavior)
	•	Automatically isolating faulty sections using relay control
	•	Restoring supply through an alternate healthy source
	•	Logging all system events and measurements
	•	Demonstrating distributed node communication using CAN bus

This system demonstrates the concept of self-healing power networks used in modern smart grids.

System Architecture Overview

The microgrid consists of three intelligent nodes:
Node and role 
Central Node
Coordination, logging, CAN management
Node B
Sensor node + relay control (primary load node)
Node C
Sensor node (secondary load node)

Each node is built around an ESP32 WROVER-B microcontroller.

Communication between nodes is established using CAN bus (MCP2515 + TJA1050) in a linear topology with proper termination.


Power System Design

Two independent 12V 10A SMPS power supplies are used to simulate dual sources.

Each node includes a carefully designed power entry stage

12V → Fuse → Schottky Diode → TVS Diode → 220µF Capacitor → Buck Converter → 5V → ESP32

<img width="1010" height="1280" alt="image" src="https://github.com/user-attachments/assets/835599e3-842d-49c4-b196-4c8c2553b432" />


This ensures:
	•	Reverse polarity protection
	•	Surge protection
	•	Stable logic supply
	•	Electrical safety


  Sensor Strategy

  A hybrid sensing approach is used to balance accuracy and response time.

  Location of sensor and purpose of them
Source A
INA226
Voltage, current, power monitoring

Source B
INA226
Voltage, current, power monitoring

Node B Load
ACS758 Hall sensor
Fast overcurrent detection

Node C load
ACS758 Hall sensor
Fast overcurrent detection

Node B Load
ADS1115
Precision analog conversion

This combination allows:
	•	Accurate source health monitoring
	•	Fast fault detection at load level
	•	Reliable data for self-healing decisions

  Communication Network (CAN Bus)

A structured CAN ID allocation strategy is used based on message priority


ID Range and purpose 

0x100–0x1FF
Control commands from central node

0x200–0x2FF
Sensor data from nodes

0x300–0x3FF
Fault messages (highest priority)

0x400–0x4FF
Logging/debug information

Termination resistors (120Ω) are placed only at the ends of the bus (Node B and Node C)


-------------------------------------------------------------------------------------------

Load Preparation

A resistive load bank using 10W ceramic resistors has been constructed to simulate controlled load conditions.
This allows safe and repeatable fault injection for testing self-healing behavior.
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/1509db03-8b73-43e5-94e2-0e66f91d6ee9" />


Hardware Preparation Completed

Before sensor integration, the following work has already been completed:
	•	Stable 5V low-side power supply designed and tested
	•	Load bank constructed and verified
	•	ESP32 boards prepared and labeled (Central, Node B, Node C)
	•	Complete BOM finalized and ordered
	•	CAN ID architecture frozen
	•	I²C bus planning finalized
	•	Power protection stage finalized
	•	GitHub project structure created


Self-Healing Logic Concept

The system follows this decision sequence:
	1.	INA226 detects abnormal source condition
	2.	ACS758 detects abnormal load current
	3.	Node sends fault frame over CAN
	4.	Central node decides isolation strategy
	5.	Relay on Node B switches to alternate source
	6.	System stabilizes and logs event to SD card

 
 
  Current Status

At this stage:
	•	Hardware architecture is finalized
	•	Major components ordered
	•	Power system and load system already tested
	•	Ready to begin sensor and CAN integration upon component arrival

<img width="1010" height="1280" alt="image" src="https://github.com/user-attachments/assets/fb9a8806-2664-4ace-be9a-4866d8bfdfb0" />

  <img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/2cd1b1a7-6675-42cd-a0c2-54e96dcb6d17" />



Expected Outcome

The final system will demonstrate:
	•	Distributed sensing
	•	CAN-based node communication
	•	Real-time fault detection
	•	Automatic recovery (self-healing)
	•	Data logging for analysis

This project reflects practical implementation of smart grid concepts using embedded systems.
