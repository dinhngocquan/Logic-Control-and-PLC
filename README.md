# PLC Experiments Automation

## Description
This repository contains PLC programming experiments for automated systems, including:
- **Car Park Control**: Manages a 10-car parking lot with entry/exit sensors and barriers.
- **Multi-Segment Conveyor Belt**: Controls 5 independent conveyor motors with product sensors.
- **Industrial Robot**: Transfers products between conveyors with precise arm control.

## Experiments

### Experiment 1: Automatic Car Park Control
**Description**: 
- Manages a parking lot with a capacity of 10 cars.
- Features entry (CBXV) and exit (CBXR) sensors to track car count.
- Barrier (TC) closes when the lot reaches capacity.

**I/O Configuration**:
- CBXV (X0): ON when a car enters.
- CBXR (X1): ON when a car exits.
- TC (Y0): ON to close barrier, OFF to open.

**Operation Test**:
1. System start: CBXV=OFF, CBXR=OFF, Car count (D2)=0, TC (Y1)=0.
2. Car enters: CBXV=ON, Car count +1.
3. Car in lot: CBXV=OFF, Car count unchanged.
4. Car exits: CBXR=ON, Car count -1.
5. Car leaves lot: CBXR=OFF, Car count unchanged.
6. Lot full (10 cars): TC (Y1)=1.
7. Repeat for stability and edge cases.

### Experiment 5: Multi-Segment Conveyor Control
**Description**:
- Controls 5 conveyor belts (MOTOR1-MOTOR5) with independent sensors (SENSOR1-SENSOR5).
- START/STOP buttons control system operation.
- Conveyor runs with product detection and continues 30s after product leaves.

**I/O Configuration**:
- START (X0): Normally open push button.
- STOP (X6): Normally open push button.
- SENSOR1-5 (X1-X5): ON when product detected.
- MOTOR1-5 (Y1-Y5): ON to run motors.

**Operation Test**:
1. System start: All sensors OFF, press START to activate.
2. Product on Conveyor 1: SENSOR1=ON, MOTOR1 & MOTOR2 run.
3. Product moves to Conveyor 2: SENSOR2=ON, SENSOR1=OFF, MOTOR2 & MOTOR3 run, MOTOR1 stops after 30s.
4. Continues similarly for Conveyors 3-5.
5. Product leaves Conveyor 5: SENSOR5=OFF, MOTOR5 stops after 30s.
6. STOP/START cycles: System pauses/resumes.
7. Repeat for stability and edge cases.

### Experiment 6: Industrial Robot Control
**Description**:
- Robot transfers products from Conveyor 1 to Conveyor 2.
- START/STOP buttons control system.
- Robot arm grips (KV), rotates forward (QT) to Conveyor 2, releases, and reverses (QN) to Conveyor 1.

**I/O Configuration**:
- START (X0), STOP (X1): Normally open push buttons.
- CBSP1 (X2): ON when product at grip position.
- CBVT1 (X3): ON when robot at Conveyor 1.
- CBVT2 (X4): ON when robot at Conveyor 2.
- MOTOR1 (Y0): Runs Conveyor 1.
- QT (Y2): Forward rotation (not simultaneous with QN).
- QN (Y3): Reverse rotation.
- KV (Y1): Grips/releases product.

**Operation Test**:
1. System start: CBSP1=OFF, CBVT1=ON, CBVT2=OFF, MOTOR1 runs (Y0=1).
2. Product at grip: CBSP1=ON, MOTOR1 stops (Y0=0), grip (Y1=1).
3. Robot rotates forward: CBVT1=OFF, QT (Y2=1), MOTOR1 runs, CBSP1=OFF.
4. Reaches Conveyor 2: CBVT2=ON, release (Y1=0), QT (Y2=0).
5. Robot rotates back: CBVT2=OFF, QN (Y3=1).
6. Reaches Conveyor 1: CBVT1=ON, QN (Y3=0).
7. Repeat for stability and edge cases.
