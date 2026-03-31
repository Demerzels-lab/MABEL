# MABEL - Multi Axis Balancer Electronically Levelled

[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![GitHub Stars](https://img.shields.io/github/stars/Demerzels-lab/MABEL?style=social)](https://github.com/Demerzels-lab/MABEL/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/Demerzels-lab/MABEL?style=social)](https://github.com/Demerzels-lab/MABEL/network/members)
[![Last Commit](https://img.shields.io/github/last-commit/Demerzels-lab/MABEL)](https://github.com/Demerzels-lab/MABEL/commits)
[![Contributors](https://img.shields.io/github/contributors/Demerzels-lab/MABEL)](https://github.com/Demerzels-lab/MABEL/graphs/contributors)
[![Project Status](https://img.shields.io/badge/status-active-brightgreen)](https://github.com/Demerzels-lab/MABEL/projects)
[![Hardware Platform](https://img.shields.io/badge/hardware-Raspberry%20Pi%20-orange)](https://www.raspberrypi.org/)
[![Software Platform](https://img.shields.io/badge/software-Arduino%20%7C%20Python-blue)](https://www.arduino.cc/)

<!-- Header Image -->
<div align="center">
  <img src="https://camo.githubusercontent.com/2d51d6135fbd3022c70a244adbf876568cc89e58a357c3de2c8d1f7edaad93b0/68747470733a2f2f692e696d6775722e636f6d2f63694f417253472e6a706567" alt="MABEL Robot" width="600"/>
  <p><i>MABEL - A Boston Dynamics Handle Inspired Self-Balancing Robot</i></p>
</div>

---

## Table of Contents

- [About The Project](#about-the-project)
- [Features](#features)
- [Technical Specifications](#technical-specifications)
- [Hardware Architecture](#hardware-architecture)
- [Software Architecture](#software-architecture)
- [Quick Start](#quick-start)
- [Bill of Materials](#bill-of-materials)
- [Build Instructions](#build-instructions)
- [Inverse Kinematics](#inverse-kinematics)
- [Serial Communication](#serial-communication)
- [Project Roadmap](#project-roadmap)
- [Contributing](#contributing)
- [License](#license)
- [Contact & Support](#contact--support)
- [Acknowledgments](#acknowledgments)
- [Related Projects](#related-projects)

---

## About The Project

**MABEL** (Multi Axis Balancer Electronically Levelled) is an open-source self-balancing robot project inspired by the renowned [Boston Dynamics Handle robot](https://www.youtube.com/watch?v=-7xvqQeoA8c). This project aims to create an **affordable legged balancing robot platform** that can be built on a hobby scale using readily available and inexpensive components.

### Project Vision

The goal of MABEL is to democratize advanced robotics by creating a platform that combines:
- **Legged locomotion** with active balancing capabilities
- **Variable leg length** for enhanced terrain adaptation
- **Off-road performance** through articulated joints
- **Open-source accessibility** for hobbyists and researchers

### Core Technology

MABEL integrates multiple technologies:
- **Arduino** - Handles PID calculations for real-time balancing based on MPU-6050 sensor data
- **Raspberry Pi** - Manages high-level control, Bluetooth connectivity, and inverse kinematics
- **Stepper Motors** - Provides precise wheel positioning and control
- **Servo System** - Enables articulated leg movement with 6 MG996R metal gear servos

This project builds upon the open-source [YABR](http://www.brokking.net/yabr_main.html) (Yet Another Balancing Robot) firmware, extending it with additional functionality for legged control.

> **Quote:** "The goal of MABEL is to create an affordable legged balancing robot platform like the Boston Dynamics Handle robot that can be built on a hobby scale using cheap Amazon parts and components."

---

## Features

### Key Capabilities

| Feature | Description |
|---------|-------------|
| **Active Multi-Axis Balancing** | Advanced PID control system maintains equilibrium using real-time sensor fusion |
| **Articulated Legs** | Six-degree-of-freedom leg system with upper and lower segments |
| **Inverse Kinematics** | Precise leg positioning in 2D Cartesian coordinates (x, y) |
| **Center of Mass Control** | Dynamic CoM manipulation for acceleration and braking |
| **Wireless Control** | Bluetooth connectivity with Wiimote/PS4 controller support |
| **Computer Vision Ready** | Raspberry Pi enables CV applications and AI integration |
| **Modular Design** | 3D printable components with plug-and-play electronics |
| **Cost-Effective** | Built with affordable Amazon/Ebay components |

### What Makes MABEL Different

MABEL stands out from traditional balancing robots through:

1. **Movable Legs** - Enhanced mobility, terrain adaptation, and stabilization
2. **Variable Geometry** - Adjustable leg positions for different environments
3. **Dual Processing** - Arduino for real-time control, Pi for high-level functions
4. **Inverse Kinematics Engine** - Mathematical precision in leg movement
5. **Open Architecture** - Easily extensible for research and experimentation

---

## Technical Specifications

### Mechanical Parameters

| Parameter | Value |
|-----------|-------|
| Upper Leg Length | 92mm |
| Lower Leg Length | 75mm |
| Vertical Range (X) | 98mm - 160mm |
| Horizontal Range (Y) | -50mm to +50mm |
| Total Height (Extended) | ~320mm |
| Total Height (Retracted) | ~258mm |

### Electronic Components

| Component | Specification |
|-----------|---------------|
| Microcontroller | Arduino Uno |
| Single Board Computer | Raspberry Pi Zero W |
| Servo Controller | PCA9865 (16-channel) |
| Inertial Measurement Unit | MPU-6050 (Gyro + Accelerometer) |
| Servos | 6x MG996R Metal Gear |
| Stepper Motors | 2x NEMA 17 (38mm) |
| Stepper Drivers | 2x A4988 or DRV8825 |
| Battery | 11.1V 2800mAh 3S LiPo |
| Voltage Regulation | 5V (Pi/Arduino), 7.4V (Servos) |

### Software Stack

```
+------------------------------------------+
|          High-Level Control (Pi)          |
|  +-------------+  +--------------------+ |
|  |  Bluetooth  |  | Inverse Kinematics | |
|  |   Control   |  |      Engine        | |
|  +-------------+  +--------------------+ |
|  +-------------+  +--------------------+ |
|  |  ServoKit  |  |    Serial Comm     | |
|  |   Driver   |  |    (pySerial)      | |
|  +-------------+  +--------------------+ |
+------------------------------------------+
                      |
                      | USB Serial
                      v
+------------------------------------------+
|         Low-Level Control (Arduino)      |
|  +-------------+  +--------------------+ |
|  |    PID      |  |     MPU-6050       | |
|  | Controller  |  |   Sensor Fusion   | |
|  +-------------+  +--------------------+ |
|  +-------------+  +--------------------+ |
|  |   Motor     |  |    Stepper        | |
|  |   Driver    |  |   Control         | |
|  +-------------+  +--------------------+ |
+------------------------------------------+
```

---

## Hardware Architecture

### System Block Diagram

```
                    +-----------------+
                    |   11.1V LiPo    |
                    |   2800mAh      |
                    +--------+--------+
                             |
            +----------------+----------------+
            |                |                |
            v                v                v
    +---------------+ +---------------+ +---------------+
    |  7.4V Reg     | |   5V Reg      | |  Stepper     |
    | (Servos)      | | (Pi + Arduino)| |  Motors      |
    +-------+-------+ +-------+-------+ +---------------+
            |                 |
            vv
    +---------------+ +---------------+
    |   PCA9865     | |   Arduino     |
    |   Servo       | |     Uno       |
    |   Controller  | |               |
    +-------+-------+ +-------+-------+
            |                 |
            |    USB Serial   |
            |<----------------+
            |                 |
            v                 v
    +---------------------------------------+
    |         Raspberry Pi Zero W           |
    |  +---------+  +---------------------+|
    |  |Bluetooth|  |   WiFi Connectivity  ||
    |  | Module  |  |   SSH / VNC Access  ||
    |  +---------+  +---------------------+|
    |  +---------------------------------+ |
    |  |  Python Control Application    ||
    |  +---------------------------------+ |
    +---------------------------------------+
```

### Leg Kinematics Structure

```
        +-------------------------------------+
        |            Body (Torso)            |
        |   +---------------------------+     |
        |   |    Hip Servo (x2)        |     |
        |   +-----------+---------------+   |
        |               |                     |
        |               v                     |
        |   +---------------------------+     |
        |   |   Upper Leg Segment       |     |
        |   |   Length: 92mm           |     |
        |   +-----------+---------------+   |
        |               |                     |
        |               v                     |
        |   +---------------------------+     |
        |   |   Knee Servo (x2)         |     |
        |   +-----------+---------------+   |
        |               |                     |
        |               v                     |
        |   +---------------------------+     |
        |   |   Lower Leg Segment       |     |
        |   |   Length: 75mm           |     |
        |   +-----------+---------------+   |
        |               |                     |
        |               v                     |
        |   +---------------------------+     |
        |   |   NEMA 17 Stepper Motor   |   |
        |   |   + 3D Printed Wheel       |   |
        |   +---------------------------+     |
        +-------------------------------------+
```

---

## Software Architecture

### Arduino Firmware (PID Control)

The Arduino runs a modified version of the YABR firmware with the following responsibilities:

1. **Sensor Fusion** - Combines MPU-6050 gyro and accelerometer data
2. **PID Control Loop** - Maintains balance at ~100Hz
3. **Motor Control** - Generates step pulses for A4988 drivers
4. **Serial Communication** - Receives commands from Raspberry Pi

```cpp
// Simplified PID Control Loop (Based on YABR)
void loop() {
    // Read angle from MPU-6050
    angle = readMPU6050();
    
    // Calculate PID output
    pid_output = pid.calculate(angle, setpoint);
    
    // Adjust motor speed based on PID output
    setMotorSpeed(pid_output);
    
    // Check for serial commands from Pi
    if (serial.available()) {
        handleSerialCommand(serial.read());
    }
}
```

### Raspberry Pi Application (Python)

The Pi handles high-level control:

1. **Bluetooth Manager** - Interfaces with wireless controllers
2. **Servo Controller** - Sends commands to PCA9865 via I2C
3. **Inverse Kinematics Engine** - Calculates leg joint angles
4. **Serial Bridge** - Communicates with Arduino

### Inverse Kinematics Engine

MABEL uses cosine rule and trigonometry to calculate joint angles:

```
Given: Target position (x, y) relative to hip pivot
      Upper leg length (a = 92mm)
      Lower leg length (b = 75mm)

Calculate: t1 (Hip angle) and t2 (Knee angle)

For Hip Servo (t1):
  B = sqrt(x^2 + y^2)
  cos(B_angle) = (a^2 + B^2 - b^2) / (2 x a x B)
  t1 = atan2(y, x) - B_angle

For Knee Servo (t2):
  cos(C_angle) = (a^2 + b^2 - B^2) / (2 x a x b)
  t2 = 180 deg - C_angle
```

---

## Quick Start

### Prerequisites

- Raspberry Pi Zero W with Raspberry Pi OS
- Arduino IDE installed on your computer or Pi
- Python 3.7+ on the Raspberry Pi

### Installation Steps

1. **Clone the Repository**
   ```bash
   git clone https://github.com/Demerzels-lab/MABEL.git
   cd MABEL
   ```

2. **Install Raspberry Pi Dependencies**
   ```bash
   # Install ServoKit library
   pip3 install adafruit-circuitpython-servokit
   
   # Install pySerial for Arduino communication
   pip3 install pyserial
   
   # Optional: Install controller library
   pip3 install approxeng.input
   ```

3. **Upload Arduino Firmware**
   - Open `arduino_code/MABEL_PID/MABEL_PID.ino` in Arduino IDE
   - Select Board: Arduino Uno
   - Upload the sketch to your Arduino

4. **Connect Hardware**
   - Connect Arduino to Raspberry Pi via USB
   - Connect PCA9865 servo controller to Pi I2C pins
   - Connect all servos and stepper drivers

5. **Run the Control Script**
   ```bash
   cd raspi_code
   python3 main.py
   ```

---

## Bill of Materials

### 3D Printable Parts

All STL files are located in [`/CAD/3D Models (To Print)`](https://github.com/Demerzels-lab/MABEL/tree/master/CAD/3D%20Models%20(To%20print))

| Part | Quantity | Description |
|------|----------|-------------|
| `BodyPanel.stl` | 2 | Side panels with servo mounts |
| `BodyTop.stl` | 1 | Upper body housing |
| `Housing.stl` | 1 | Lower body housing |
| `UpperLeg.stl` | 2 | Upper leg segments |
| `LowerLeg.stl` | 2 | Lower leg segments |
| `DriverGear.stl` | 4 | Gear assemblies for joints |
| `Wheel.stl` | 2 | 3D printed wheel (optional) |
| `BatteryPack.stl` | 1 | LiPo battery mount |
| `PanBracketHead.stl` | 1 | Pan servo bracket (optional) |
| `TiltBracketHead.stl` | 1 | Tilt servo bracket (optional) |

### Electronic Components

| Component | Quantity | Notes |
|-----------|----------|-------|
| Raspberry Pi Zero W | 1 | Main computer |
| Arduino Uno | 1 | PID controller |
| PCA9865 Servo Controller | 1 | 16-channel I2C |
| MG996R Servos | 6 | Metal gear servos |
| NEMA 17 Stepper Motors | 2 | 38mm length |
| A4988/DRV8825 Drivers | 2 | Stepper motor drivers |
| MPU-6050 | 1 | Gyro/Accelerometer |
| 11.1V 2800mAh 3S LiPo | 1 | Main battery |
| Voltage Regulators | 2 | 5V and 7.4V |
| CNC Shield (Arduino) | 1 | Motor driver shield |

### Mechanical Hardware

| Item | Quantity | Size |
|------|----------|------|
| Bearings | 8 | 10mm OD, 5mm ID |
| M3 Bolts | 12 | 10mm |
| M4 Bolts | 16 | 15mm |
| M5 Bolts | 4 | 30mm |
| M5 Bolts | 12 | 15mm |
| M5 Locknuts | 20 | With washers |
| Aluminium Servo Horns | 6 | For MG996R |

### Optional: Amazon Wishlist

For convenience, a precompiled shopping list is available:
[Amazon Wishlist](https://www.amazon.co.uk/gp/registry/wishlist/1K7SOU8MRG2K7)

---

## Build Instructions

### Step 1: Prepare 3D Printed Parts

Print all STL files with the following recommended settings:
- **Layer Height**: 0.2mm
- **Infill**: 20-40%
- **Material**: PLA or PETG recommended
- **Supports**: Required for overhangs

### Step 2: Assemble Legs

#### 2.1 Press Bearings into Joints
- Insert 2 bearings per leg section (8 total)
- Use a bench vice with even pressure
- Optionally soften plastic with a hairdryer

#### 2.2 Attach Servos to Panels
- Mount 4 MG996R servos to BodyPanel and UpperLeg parts
- Use M4 (15mm) bolts - 16 total
- Ensure servo shafts face outward

#### 2.3 Install Driver Gears
- Push servo horns into DriverGear recesses
- Secure with provided screws
- Attach gear assemblies to servo shafts

#### 2.4 Mount Stepper Motors
- Secure NEMA 17 motors to LowerLeg parts
- Use M3 (10mm) bolts - 8 total
- Attach wheels to motor shafts

#### 2.5 Complete Leg Assembly
- Connect UpperLeg to BodyPanel with M5 locknuts
- Connect LowerLeg to UpperLeg with M5 locknuts
- **Important**: Align servo positions for full range of motion

### Step 3: Electronics Assembly

1. **Arduino Setup**
   - Mount Arduino Uno in upper body
   - Install CNC Shield with A4988 drivers
   - Connect MPU-6050 to I2C pins

2. **Raspberry Pi Setup**
   - Mount Pi Zero W in upper body
   - Connect PCA9865 to Pi I2C pins (SDA/SCL)
   - Wire USB connection to Arduino

3. **Power Distribution**
   - Connect 7.4V regulator to servo power rail
   - Connect 5V regulator to Pi and Arduino
   - Connect LiPo battery to main power input

### Step 4: Calibration

1. **Servo Zero Calibration**
   - Disconnect gears from servos
   - Center all servos to 90 deg
   - Mark the 0 deg position for each servo
   - Record home positions in `IKSolve.py`

2. **MPU-6050 Calibration**
   - Upload YABR firmware to Arduino
   - Use serial monitor to view angle data
   - Calibrate offset values for level position

3. **PID Tuning**
   - Start with YABR default values
   - Tune Kp, Ki, Kd for smooth balancing
   - Test and iterate until stable

---

## Inverse Kinematics

### Using the IKSolve Class

```python
from IKSolve import IKSolve

# Initialize with your servo home positions
# (Values will differ based on your calibration)
ik_handler = IKSolve(
    ru_home=50,   # Right upper servo home
    rl_home=109,  # Right lower servo home
    lu_home=52,   # Left upper servo home
    ll_home=23    # Left lower servo home
)

# Move legs to target position (in mm)
x = 120  # Vertical distance (98-160mm)
y = 25   # Horizontal distance (-50 to +50mm)

# Get servo angles for target position
servo_angles = ik_handler.translate_xy(x, y, flip=False)

# servo_angles[0] = Right upper servo angle
# servo_angles[1] = Right lower servo angle
# servo_angles[2] = Left upper servo angle
# servo_angles[3] = Left lower servo angle
```

### Movement Demo

```python
from IKSolve import IKSolve
import time

ik = IKSolve(ru_home=50, rl_home=109, lu_home=52, ll_home=23)

# Squat down
servo_angles = ik.translate_xy(98, 0)
print(f"Squat position: {servo_angles}")
time.sleep(1)

# Stand up
servo_angles = ik.translate_xy(160, 0)
print(f"Stand position: {servo_angles}")
time.sleep(1)

# Lean forward
servo_angles = ik.translate_xy(120, 50)
print(f"Lean forward: {servo_angles}")
```

---

## Serial Communication

### Protocol Overview

MABEL uses serial communication between Raspberry Pi and Arduino:

| Byte | Command | Action |
|------|---------|--------|
| `0b00000000` | STOP | Stop all movement |
| `0b00000001` | LEFT | Turn left |
| `0b00000010` | RIGHT | Turn right |
| `0b00000100` | FORWARD | Move forward |
| `0b00001000` | BACKWARD | Move backward |

### Python Implementation

```python
import serial
from struct import pack

# Initialize serial connection
ser = serial.Serial(
    port='/dev/ttyACM0',  # Find your port with: ls /dev/tty*
    baudrate=9600,
    parity=serial.PARITY_NONE,
    stopbits=serial.STOPBITS_ONE,
    bytesize=serial.EIGHTBITS,
    timeout=1
)

def send_command(direction):
    """Send movement command to Arduino"""
    command = {
        'stop': 0b00000000,
        'left': 0b00000001,
        'right': 0b00000010,
        'forward': 0b00000100,
        'backward': 0b00001000
    }
    ser.write(pack("B", command[direction]))

# Example usage
send_command('forward')  # Start moving forward
time.sleep(2)
send_command('stop')     # Stop
```

---

## Project Roadmap

### Completed Features
- [x] Basic balancing PID controller
- [x] Inverse kinematics engine
- [x] Serial communication protocol
- [x] Bluetooth controller support
- [x] 3D printable components
- [x] Build documentation

### In Development
- [ ] Improved controller smoothing
- [ ] Vertical leg movement stability
- [ ] Self-righting mechanism
- [ ] Camera mount integration

### Future Plans
- [ ] Computer vision integration
- [ ] SLAM navigation
- [ ] Mobile app control
- [ ] IMU-based gesture control
- [ ] Autonomous obstacle avoidance
- [ ] Machine learning for gait optimization

---

## Contributing

Contributions are welcome! Here's how you can help:

1. **Fork the Repository**
   ```bash
   git fork https://github.com/Demerzels-lab/MABEL
   ```

2. **Create a Feature Branch**
   ```bash
   git checkout -b feature/amazing-feature
   ```

3. **Commit Your Changes**
   ```bash
   git commit -m 'Add amazing feature'
   ```

4. **Push to the Branch**
   ```bash
   git push origin feature/amazing-feature
   ```

5. **Open a Pull Request**

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct and development process.

---

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.

### What This Means
- You may freely use, modify, and distribute this project
- You must disclose source code when distributing
- You must keep the same license
- You may patent improvements, but must allow free use

---

## Contact & Support

### Project Links

| Platform | Link |
|----------|------|
| **GitHub** | [Demerzels-lab/MABEL](https://github.com/Demerzels-lab/MABEL) |
| **Original Repo** | [raspibotics/MABEL](https://github.com/raspibotics/MABEL) |
| **Hackaday.io** | [Project Page](https://hackaday.io/project/174129-mabel-a-boston-dynamics-inspired-balancing-robot) |
| **Build Blog** | [raspibotics Blog](https://raspibotics.wixsite.com/pibotics-blog) |

### Community & Support
- **GitHub Issues**: Report bugs and request features
- **Discussions**: Ask questions and share ideas
- **Twitter**: [@raspibotics](https://twitter.com/raspibotics)

### Contact Email
- **General Inquiries**: [raspibotics@gmail.com](mailto:raspibotics@gmail.com)

---

## Acknowledgments

### Open Source Projects

Special thanks to these projects that made MABEL possible:

| Project | Author | Description |
|---------|--------|-------------|
| [YABR](http://www.brokking.net/yabr_main.html) | Joop Brokking | Base PID balancing firmware |
| [Adafruit ServoKit](https://github.com/adafruit/Adafruit_CircuitPython_ServoKit) | Adafruit | Python servo control library |
| [approxeng.input](https://approxeng.github.io/approxeng.input/) | Approxeng | Cross-platform controller support |

### Inspiration
- [Boston Dynamics Handle](https://www.youtube.com/watch?v=-7xvqQeoA8c) - Robot design inspiration
- [Hackaday Prize 2020](https://hackaday.io/project/174129) - Contest participation

### CAD & 3D Printing Community
- OpenSCAD and FreeCAD contributors
- r/3Dprinting and r/raspberry_pi communities

---

## Related Projects

### Similar Open Source Robots

| Project | Platform | Focus |
|---------|----------|-------|
| [YABR](http://www.brokking.net/yabr_main.html) | Arduino | Self-balancing robot |
| [Stanford Pupper](https://github.com/stanfordroboticsclub/Stanford_Pupper) | Raspberry Pi | Quadruped robot |
| [OpenDog](https://github.com/XRobots/OpenDog) | Various | Quadruped robot |

### Recommended Reading
- "Robot Modeling and Control" by Spong, Hutchinson, Vidyasagar
- "Modern Robotics" by Lynch and Park (Free online)
- PID Tuning guides for balancing robots

---

<div align="center">

**Built with heart by the MABEL community**

[![Stargazers](https://img.shields.io/github/stars/Demerzels-lab/MABEL?style=social)](https://github.com/Demerzels-lab/MABEL/stargazers)
[![Forks](https://img.shields.io/github/forks/Demerzels-lab/MABEL?style=social)](https://github.com/Demerzels-lab/MABEL/network/members)

**Last Updated**: 2026
**Version**: 1.0.0

</div>
