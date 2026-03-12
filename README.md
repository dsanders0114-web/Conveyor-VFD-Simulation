# Overview

This project demonstrates a simulated conveyor control system built in **Studio 5000 Logix Designer**.
The application models a conveyor driven by a variable frequency drive (VFD) using a simplified **EtherNet/IP command and feedback structure**.

The PLC program includes structured routines for:

* machine permissives
* operator control
* VFD command generation
* VFD behavior simulation
* conveyor process modeling
* centralized fault management

The system runs entirely in a **virtual PLC environment**, making it suitable for learning and demonstration without physical hardware.

---

# System Architecture

The PLC program is organized into modular routines.

```
MainRoutine
│
├── R_Permissives
│
├── R_OperatorControl
│
├── R_VFD_Command
│
├── R_VFD_Simulation
│
├── R_Conveyor_Model
│
└── R_Faults
```

Each routine has a specific responsibility to keep the program readable and scalable.

---

# Control Strategy

The simulated system follows this sequence:

```
Operator command
      ↓
Run permissive check
      ↓
Run request generated
      ↓
VFD command word created
      ↓
VFD simulation calculates speed ramp
      ↓
Conveyor model calculates load
      ↓
Fault routine monitors system health
```

---

# VFD Simulation Model

The drive simulation uses a simplified EtherNet/IP style interface.

### Command Word

| Bit | Function    |
| --- | ----------- |
| 0   | Stop        |
| 1   | Start       |
| 3   | Fault Reset |
| 4   | Forward     |

### Status Word

| Bit | Meaning  |
| --- | -------- |
| 0   | Ready    |
| 1   | Active   |
| 2   | Forward  |
| 7   | Fault    |
| 8   | At Speed |

### Speed Scaling

Drive reference and feedback use scaled values.

```
Reference = Speed (Hz) × 100
Feedback  = Speed (Hz) × 100
```

Example:

```
30 Hz = 3000
```

---

# Conveyor Simulation Model

The conveyor process includes:

### Load Modeling

Base load is always present.

Additional load is added when:

* product is detected
* conveyor operates at high speed

### Overload Protection

If conveyor load exceeds a threshold for several seconds, an overload fault is triggered.

---

# Fault Handling

Fault conditions monitored by the system include:

| Fault               | Description                              |
| ------------------- | ---------------------------------------- |
| Conveyor Overload   | Load exceeds safe operating range        |
| Drive Fault         | Simulated drive failure                  |
| Communication Fault | Simulated EtherNet/IP communication loss |

Fault reset is performed by the operator using the reset command.

---

# Tags Used

Example tags used in the simulation.

```
Cmd_Run
Conv_SpeedCmd_Hz
Conv_SpeedFb_Hz
Conv_RunFb
Conv_LoadPct
Conv_ProductPresent

VFD_LogicCommand
VFD_Reference
VFD_Feedback
VFD_LogicStatus

System_Fault
Sim_Overload
Sim_DriveFault
Sim_CommFault
```

---

# How to Run the Simulation

1. Open the project in **Studio 5000 Logix Designer**.
2. Download to a **Logix Emulate controller** or virtual controller.
3. Monitor tags in the controller tag table.

Suggested test procedure:

```
PB_EStop_OK = TRUE
PB_GuardDoor_OK = TRUE
Mode_Auto = TRUE

Cmd_Speed_Hz = 30

Press PB_Start
```

Expected behavior:

* speed ramps to 30 Hz
* conveyor running bit turns on
* status word updates
* load calculation begins

---

# Example Test Scenarios

### Normal Operation

```
Command speed = 30 Hz
```

Expected result:

```
Speed ramps up
Drive Active = TRUE
AtSpeed = TRUE
```

---

### Conveyor Overload

Force:

```
Conv_ProductPresent = TRUE
```

Expected result:

```
Load increases
Overload timer triggers
System fault activates
```

---

### Communication Fault

Force:

```
Sim_CommFault = TRUE
```

Expected result:

```
System fault activates
Drive run request removed
Conveyor decelerates
```

---

# Skills Demonstrated

This project demonstrates several industrial automation concepts:

* PLC structured program design
* EtherNet/IP device modeling
* VFD command and status interface
* speed ramp simulation
* conveyor process modeling
* fault detection and handling
* ladder logic documentation

---


# Files Included

```
/Studio5000_Project
Conveyor_VFD_Simulation.ACD
```

---

# Author

```
Daryl Sanders
Automation / Controls Portfolio Project
```

