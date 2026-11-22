
# PROJECT OVERVIEW
## Smart Classroom Digital Twin System

### 1. Project Background

Classroom management is an important aspect of creating an effective and comfortable learning environment. However, conventional classroom management still faces several challenges, such as limited environmental monitoring, inefficient electricity usage, and the lack of centralized classroom management.

To address these challenges, the **Smart Classroom Digital Twin System** is proposed as an IoT-based platform that integrates physical classroom conditions with a virtual 3D representation.

The system utilizes IoT sensors to collect environmental data and synchronizes the information with a virtual classroom model. Through this integration, users can monitor classroom conditions, analyze energy consumption, simulate environmental changes, and control connected classroom devices through a centralized dashboard.

### 2. Problem Statement

The main problems identified in conventional classroom management are:

1. Limited ability to monitor classroom environmental conditions in real time.
2. Inefficient electricity consumption due to lights and fans remaining active when classrooms are unoccupied.
3. Lack of centralized monitoring for classroom facilities.
4. Difficulty identifying changes in classroom environmental conditions.
5. Absence of an interactive virtual representation of the physical classroom.

### 3. Project Objectives

The objectives of developing this system are:

1. To develop a virtual 3D representation of a physical classroom.
2. To monitor classroom environmental conditions in real time.
3. To detect classroom occupancy using IoT sensors.
4. To monitor electrical energy consumption.
5. To provide remote control of connected classroom devices.
6. To simulate classroom environmental conditions through a virtual environment.
7. To support more efficient classroom management through data analysis.

### 4. Project Scope

The system will cover the following functionalities:

**A. Environmental Monitoring**
- Temperature monitoring.
- Humidity monitoring.
- Light intensity monitoring.
- Classroom occupancy detection.

**B. Digital Twin Visualization**
- 3D virtual classroom representation.
- Real-time synchronization with IoT sensor data.
- Visualization of classroom device conditions.
- Virtual representation of classroom environmental changes.

**C. Smart Device Management**
- Remote lighting control.
- Fan control.
- Monitoring connected device status.

**D. Energy Monitoring**
- Monitoring electrical energy consumption.
- Displaying energy usage statistics.
- Providing historical energy consumption data.

**E. Simulation System**
- Simulating changes in classroom temperature.
- Simulating occupancy conditions.
- Simulating device status changes.

### 5. Target Users

The system is designed for the following users:

| User | Responsibilities |
|---|---|
| Administrator | Manages the system, users, and classroom devices. |
| Teacher | Monitors classroom conditions and controls available devices. |
| School Management | Reviews classroom conditions and energy consumption reports. |

### 6. Technology Overview

The proposed technologies include:

| Component | Technology |
|---|---|
| Frontend | React.js |
| 3D Visualization | Three.js |
| Backend | Node.js and Express.js |
| Database | MySQL / PostgreSQL |
| Microcontroller | ESP32 |
| Communication Protocol | MQTT |
| Temperature & Humidity | DHT22 Sensor |
| Occupancy Detection | PIR Sensor |
| Light Intensity | LDR Sensor |
| Energy Monitoring | PZEM-004T |

### 7. System Architecture Overview

The system consists of the following main components:

1. **Physical Classroom:** The real classroom equipped with sensors and connected devices.
2. **IoT Layer:** Collects environmental data through ESP32.
3. **Communication Layer:** Transfers sensor data using MQTT.
4. **Backend Layer:** Processes sensor data and device control requests.
5. **Database Layer:** Stores sensor readings, device information, and historical records.
6. **Digital Twin Layer:** Represents the physical classroom in a virtual 3D environment.
7. **User Interface:** Provides a centralized dashboard for monitoring and controlling the system.

### 8. Expected Outcomes

The expected outcomes of this project include:

- An interactive web-based classroom management dashboard.
- A virtual 3D classroom synchronized with physical classroom conditions.
- Real-time environmental monitoring.
- Smart classroom device control.
- Energy consumption monitoring and reporting.
- A simulation environment for classroom condition analysis.

### 9. Project Limitations

The initial development focuses on a limited classroom environment.

The following limitations are identified:

1. The initial implementation focuses on one physical classroom.
2. Monitoring capabilities depend on the availability and accuracy of IoT sensors.
3. Device control is limited to supported connected hardware.
4. The 3D virtual model represents selected classroom objects and conditions.
5. Predictive analysis and advanced AI features are considered future development.

### 10. Project Conclusion

The Smart Classroom Digital Twin System aims to transform conventional classroom management into an intelligent and integrated environment.

By combining Internet of Things (IoT), Digital Twin technology, and web-based applications, the system provides a foundation for real-time monitoring, virtual simulation, and more efficient classroom management.

