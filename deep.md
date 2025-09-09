
# Autonomous Drone Control with ROS 2

A comprehensive system for autonomous drone control using ROS 2 Jazzy, PX4, Gazebo Harmonic, and Rust.

## 🎯 Project Objectives

- Implement a Finite State Machine (FSM) for autonomous drone control
- Achieve autonomous takeoff, waypoint navigation, and landing
- Execute complex maneuvers (figure-8 pattern)
- Collect and log telemetry data
- Ensure safe operation with failsafe handling

## 🛠️ Technology Stack

- **Operating System**: Ubuntu 24.04 Noble
- **Robotics Framework**: ROS 2 Jazzy
- **Simulation**: Gazebo Harmonic
- **Flight Controller**: PX4 Autopilot
- **Programming Language**: Rust
- **Ground Control**: QGroundControl
- **Communication**: Micro XRCE-DDS Agent

## 📋 Prerequisites

1. **Ubuntu 24.04 Noble**
   - [Download Ubuntu 24.04](https://ubuntu.com/download/desktop)

2. **ROS 2 Jazzy**
   - [Installation Guide](https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html)

3. **Gazebo Harmonic**
   - [Installation Guide](https://gazebosim.org/docs/harmonic/install_ubuntu/)

4. **PX4 Autopilot**
   - [Build Instructions](https://docs.px4.io/main/en/dev_setup/building_px4)

5. **Additional Components**
   - [PX4 ROS 2 Messages](https://github.com/PX4/px4_msgs)
   - [Micro XRCE-DDS Agent](https://docs.px4.io/main/en/middleware/uxrce_dds.html)
   - [QGroundControl](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html)

## 🚀 Getting Started

To begin working with the drone simulation environment:

1. First, launch Gazebo Harmonic with the PX4 drone model
2. Open additional terminals to start the Micro XRCE-DDS Agent and QGroundControl
3. Use PX4's command-line interface for initial manual control testing

This initial setup helps verify that all components are working correctly before moving on to autonomous control.

---


### System Launch

Launch these components in separate terminals:

```bash
# Terminal 1: PX4 SITL with Gazebo
make px4_sitl gz_x500

# Terminal 2: Micro XRCE-DDS Agent
MicroXRCEAgent udp4 -p 8888

# Terminal 3: QGroundControl
~/Downloads/QGroundControl-x86_64.AppImage
```

### Basic PX4 Commands

Test basic functionality using PX4 shell commands:
```bash
commander arm
commander takeoff
commander land
commander disarm
```

After verifying the basic functionality, we proceeded with the Rust integration by identifying and configuring the necessary dependencies in the project's `Cargo.toml` file.

## 💻 Development

### Project Dependencies

```toml
[package]
name = "drone_control"
version = "0.1.0"
edition = "2021"

[dependencies]
rclrs = "0.5"
anyhow = "1"
px4_msgs = { path = "../../install/px4_msgs/share/px4_msgs/rust" }
ctrlc = "3.4"
chrono = "0.4"
csv = "1.1"

[patch.crates-io]
builtin_interfaces = { path = "../../../workspace/install/builtin_interfaces/share/builtin_interfaces/rust" }
```

### Features Implemented

#### Core Functionality

The development process followed an incremental approach to implement drone control features in Rust:

**Phase 1: Basic Controls**
- Vehicle arming and disarming
- Takeoff and landing procedures
- Safety checks and state monitoring

**Phase 2: Navigation**
- Waypoint navigation in NED frame
- Position hold capabilities
- Altitude control

**Phase 3: Advanced Maneuvers**
- Custom flight patterns
- Figure-8 trajectory execution
- Smooth transition between waypoints

## Telemetry Logging

I also worked on telemetry:

    Printing the drone’s state and position in the terminal 

    Attempting to log telemetry to CSV files using the csv crate  (not fully solved yet)


## Failsafe Handling

One challenge I couldn’t fully solve (yet):

    Handling failures such as Offboard mode timeout or invalid position estimate.

    The requirement was to trigger a safe land in these cases.

    My node doesn’t fully handle this yet — an area for improvement.

## Achievements

    Built a working ROS 2 + PX4 + Rust integration

    Controlled the drone fully without QGroundControl (only for monitoring)

    Implemented FSM logic for mission phases

    Achieved a figure-8 maneuver
