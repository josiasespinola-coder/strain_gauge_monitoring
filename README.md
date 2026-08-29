# 📡 Wireless Strain Gauge Monitoring System

*Note: This project was developed for a private company. Due to intellectual property and NDA agreements, the source code, full schematics, and Gerber files are not publicly available. This repository serves as a technical case study of the system architecture and my engineering contributions.*

## 📋 Project Overview
A rugged, battery-powered industrial IoT system designed for real-time measurement of torque, thrust force, and RPM on rotating shafts. The system acquires data directly from the moving components and securely transmits it to a centralized monitoring dashboard.

## 🏗️ System Architecture
```graph LR
    subgraph "Rotating Shaft Node (Battery Powered)"
        SG[Strain Gauge] -->|Analog Signal| ADC[HX711 24-bit ADC]
        ADC -->|Digital Data| MCU[XIAO nRF52840 Sense]
        Batt[Battery System] --> MCU
    end
    
    subgraph "Static Infrastructure"
        Relay[BLE to LoRa Relay]
        GW[Central Dashboard/Gateway]
    end

    MCU -.->|BLE (Ultra-Low Power)| Relay
    Relay -.->|LoRa (Long Range)| GW```

The hardware relies on a distributed wireless architecture to avoid wiring on rotating machinery:
- **Sensor Node:** Uses a XIAO nRF52840 Sense combined with an HX711 ADC for high-precision strain gauge reading.
- **Relay Node:** Captures BLE packets from the rotating shaft and transmits them over long distances via LoRa to the main industrial gateway.

## 💡 Key Engineering Challenges & Solutions

### 1. Ultra-Low Power Optimization
Operating on a battery in a continuous rotating environment meant power efficiency was critical. 
- **Solution:** I implemented aggressive sleep states on the nRF52840, waking up only for fast ADC sampling and brief BLE broadcast bursts. 
- **Result:** Successfully achieved a calculated and field-verified battery life of **>2 years**.

### 2. Wireless Reliability on Moving Parts
Maintaining a stable connection on a fast-rotating metal shaft required careful RF consideration.
- **Solution:** Segregated the communication into two hops: a fast, short-range BLE link from the shaft to a static nearby relay, and a robust LoRa link from the relay to the central system.

### 3. Design for Manufacturing (DFM)
- Designed the complete schematic and PCB layout optimizing for small form factor to fit the shaft constraints.
- Managed the Bill of Materials (BOM) and coordinated the PCBA manufacturing process through JLCPCB.

## 🛠️ My Role and Technologies Used
I was the lead engineer for this specific node, responsible for the full lifecycle from concept to deployment.

- **Hardware:** Schematic capture, PCB Layout, BOM management, PCBA (JLCPCB).
- **Firmware:** Embedded C/C++, BLE stack configuration, LoRa protocol integration, Power management.
- **Components:** XIAO nRF52840 Sense, HX711 (24-bit ADC).
- **Status:** In active production and deployed in industrial environments for 6+ months without hardware failures.
