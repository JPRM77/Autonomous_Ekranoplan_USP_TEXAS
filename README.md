# Autonomous Ekranoplan Project

This repository contains the autonomous flight control system for an Ekranoplan (Wing-in-Ground-effect vehicle). The project is a collaborative research initiative between the **University of São Paulo (USP)** and **Texas A&M University**, developed at the **AeroTech Laboratory** under the supervision of **Professor Glauco Caurim**.

The system leverages ROS 2, Micro XRCE-DDS, and MAVROS to achieve high-frequency communication and precise low-altitude control within the ground effect zone using the PX4 Autopilot stack.

---

## 🛠️ System Architecture & Hardware

### 🧠 Processing & Control
* **Flight Controller**: Cube Orange (Running PX4 Autopilot)
* **OBC (On-Board Computer)**: Raspberry Pi (Running ROS 2 & Micro XRCE-DDS Agent)

### 📊 Sensors
* **Airspeed**: Digital Pitot Tube
* **Positioning**: GPS Module
* *Note: Future revisions plan to integrate an ultrasonic sensor for high-precision low-altitude ground clearance.*

### ⚡ Power Management
* **Propulsion/System Power**: 2x LiPo Batteries
* **Electronics/Avionics Power**: 1x LiFe Battery
* **Regulation**: 1x Power Module & 1x Step-Down Regulator

### 🎮 Actuators & Communication
* **Control Surfaces**: 6x Servo Motors
* **Propulsion**: 2x Brushless Motors
* **Telemetry**: 1x Telemetry Pair (Ground-to-Air data link)
* **Manual Override**: 1x Radio Receiver & Radio Controller Pair

---

## 🚀 Key Features

* **Ground Effect Optimization**: Custom PX4 tuning and ROS 2 control loops for low-altitude stability.
* **uXRCE-DDS Bridging**: Ultra-low latency native DDS communication between Cube Orange and Raspberry Pi.
* **MAVROS Telemetry**: High-level mission monitoring and override capabilities via ROS 2.

---

## 💻 Installation

### Prerequisites
Ensure your Raspberry Pi runs a compatible ROS 2 distribution (e.g., Humble) and has the required network interfaces configured.

```bash
# Install MAVROS for ROS 2
sudo apt install ros-${ROS_DISTRO}-mavros ros-${ROS_DISTRO}-mavros-extras

# Clone and install the Micro XRCE-DDS Agent
git clone https://github.com
cd Micro-XRCE-DDS-Agent && mkdir build && cd build
cmake .. && make && sudo make install
sudo ldconfig /usr/local/lib/
```

### Workspace Setup
```bash
mkdir -p ~/ekranoplan_ws/src
cd ~/ekranoplan_ws/src
git clone https://github.com

cd ~/ekranoplan_ws
rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install
```

---

## 🏁 Quick Start

### 1. Start the Micro XRCE-DDS Agent
Establish the serial or UDP connection between the Raspberry Pi and the Cube Orange:
```bash
# For serial connection (adjust device and baudrate as needed)
MicroXRCEAgent serial --dev /dev/ttyAMA0 -b 921600

# For UDP connection
MicroXRCEAgent udp4 -p 8888
```

### 2. Launch MAVROS and Ekranoplan Nodes
```bash
source ~/ekranoplan_ws/install/setup.bash
ros2 launch ekranoplan_control bringup.launch.py
```

---

## 🤝 Partners and Acknowledgments

This project is a joint effort between:
* **University of São Paulo (USP)** - Escola de Engenharia de São Carlos (EESC)
* **Texas A&M University** - Department of Aerospace Engineering
* **AeroTech Lab** - Headed by Prof. Dr. Glauco Caurim

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
