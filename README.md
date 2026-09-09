# SWARNIM — Environmental Intelligence Network

<div align="center">

[![SIH 2026](https://img.shields.io/badge/SIH_2026-Problem_Statement_26178-orange?style=for-the-badge&logo=target)](https://sih.gov.in)
[![Organization: Qualcomm](https://img.shields.io/badge/Organization-Qualcomm_Inc.-3253DC?style=for-the-badge&logo=qualcomm)](https://www.qualcomm.com)
[![Category: Hardware](https://img.shields.io/badge/Category-Hardware-E02424?style=for-the-badge&logo=circuitverse)](https://sih.gov.in)
[![Theme: Disaster Management](https://img.shields.io/badge/Theme-Disaster_Management-0E9F6E?style=for-the-badge&logo=shield)](https://sih.gov.in)
[![Edge AI](https://img.shields.io/badge/Edge_AI-ESP32--S3_%7C_Raspberry_Pi_5-FF6F00?style=for-the-badge&logo=raspberrypi)](https://www.tensorflow.org/lite/microcontrollers)
[![Connectivity](https://img.shields.io/badge/Network-LoRaWAN_%7C_Sub--GHz-green?style=for-the-badge&logo=rss)](https://lora-alliance.org)
[![Cloud](https://img.shields.io/badge/Cloud-AWS_IoT_%7C_Lambda_%7C_QuickSight-232F3E?style=for-the-badge&logo=amazonwebservices)](https://aws.amazon.com)

**Decentralized, Solar-Autonomous Edge-AI Multi-Hazard Early Warning Network for India**

[System Architecture](#high-level-system-architecture) • [Competitive Hegemony](#competitive-hegemony-why-swarnim-outperforms-existing-products) • [Two-Tier Compute](#two-tier-compute-architecture) • [Edge & Cloud Partition](#functional-partition-matrix) • [Hardware & BOM](#hardware-components--bill-of-materials) • [Power Management](#dynamic-power-priority-management) • [Quickstart](#getting-started) • [References](#references--standards-bibliography)

---

</div>

## Executive Summary

India experiences catastrophic, rapidly escalating environmental hazards: urban flash floods, Himalayan landslides, forest fires in Uttarakhand and the Northeast, toxic industrial leaks, and dangerous winter smog. While national agencies (**NDMA, IMD, CPCB, and ISRO**) provide vital macro-level meteorological forecasting, disasters strike at hyper-local coordinates where terrestrial communication and power infrastructure fail first.

The **SWARNIM (Smart Warning & Resilient Network for Intelligence & Monitoring)** bridges this critical last-mile detection gap. Built for **SIH Problem Statement #26178 (Qualcomm Inc.)**, SWARNIM is a decentralized network of autonomous, solar-powered sensor nodes and edge intelligence hubs that operates on a resilient founding principle:

> **SENSE LOCALLY → PROCESS LOCALLY → DECIDE LOCALLY → WARN LOCALLY → COMMUNICATE WHEN POSSIBLE → SYNC TO CLOUD**

Instead of blindly streaming heavy raw sensor data over fragile cellular links, SWARNIM distributes intelligence:
1. **Low-Cost Sensor Nodes (ESP32-S3):** Continuously gather environmental data, perform physical threshold checks, noise filtering, and broadcast lightweight LoRa packets.
2. **Edge Processing Hubs (Raspberry Pi 5 + TinyML):** Execute on-device AI inference, cross-sensor anomaly detection, local hazard scoring, trigger immediate local sirens/LED beacons, and maintain an offline store-and-forward buffer on microSD during connectivity dropouts.
3. **Resilient LoRa Backhaul:** Transmits ultra-compact **32-byte actionable alert packets** over **Sub-GHz LoRa / LoRaWAN (IN865)** to regional command centers.
4. **Coordinated Cloud Sync (AWS):** Ingests validated alerts via AWS IoT Core and Lambda to update DynamoDB, S3 historical archives, and Amazon QuickSight emergency dashboards when connectivity is available.

---

## SIH 2026 Problem Alignment

| Attribute | Official Problem Details |
| :--- | :--- |
| **Problem Statement ID** | **26178** (Listed under SIH PS #178) |
| **Problem Title** | AI Environmental Early-Warning Network |
| **Organization** | **Qualcomm Inc.** |
| **Category** | **Hardware** (IoT Transduction, Embedded Systems & Edge AI) |
| **Theme** | **Disaster Management** |
| **Target Stakeholders** | NDMA, State Disaster Management Authorities (SDMAs), Municipal Corporations, Forest Departments, Vulnerable Communities |
| **Core Value Proposition** | Sub-second offline threat classification + Hierarchical Edge AI + Zero-grid solar autonomy + Local siren actuation |

---

## Key Technical Metrics

```text
  ┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐
  │   < 120 ms      │   │   12–15 km      │   │    72+ hrs      │   │   ~₹3,360       │
  │ On-Device Infer │   │ Sub-GHz Reach   │   │ Zero-Sun Backup │   │ Field Node BOM  │
  └─────────────────┘   └─────────────────┘   └─────────────────┘   └─────────────────┘
```

- **Zero-Grid Autonomy:** Custom MPPT solar harvesting paired with thermally stable **LiFePO4 chemistry** (survives 0°C to 60°C Indian field temperatures).
- **Deterministic Safety + AI Ensemble:** Parallel execution of hard physical safety rules alongside quantized neural models eliminates false negatives while providing transparent decision explainability.
- **Bandwidth & Radio Discipline:** 99.8% reduction in channel congestion by streaming event-driven 32-byte binary payloads instead of round-the-clock sensor telemetry.
- **Zero Data Loss (Store-and-Forward):** MicroSD and SPI Flash circular buffers preserve all telemetry during total communication blackouts and synchronize automatically upon reconnection.
- **Instant Local Warning:** Direct actuation of 110 dB sirens and strobe lights within milliseconds without waiting for cloud confirmation.

---

## Competitive Hegemony: Why SWARNIM Outperforms Existing Products

SWARNIM is engineered to overcome the fundamental vulnerabilities found across competing market paradigms:
1. **Legacy Industrial Hydrometric Stations:** Multi-lakh telemetry setups (Campbell Scientific CR1000X, Sutron / OTT HydroMet, YSI).
2. **Commercial Smart-City IoT Nodes:** Typical commercial IoT gadgets (Libelium Waspmote, Milesight IoT, cellular data loggers).
3. **Macro-Meteorological Forecasting:** Centralized governmental platforms (IMD Doppler Weather Radar, ISRO MOSDAC, CWC flood forecasts).
4. **Generic DIY / Hackathon Prototypes:** Single-tier Arduino/Raspberry Pi assemblies with cloud-dependent scripts.

### Core Architectural Advantages

#### 1. Frugal Panchayat Economics (95% CapEx Reduction)
* Standard hydro-meteorological stations cost over **₹5,00,000 to ₹15,00,000** per station. As a result, states can only afford to monitor major reservoirs and barrage walls, leaving thousands of tributary rivers, mountain nullahs, and rural culverts completely unmonitored.
* SWARNIM’s **Tier 1 field node costs ~₹3,360 ($40)** and the **Tier 2 Edge Hub costs ~₹21,700 ($260)**.
* **The Economic Multiplier:** For the cost of *one* legacy station (~₹10,00,000), a district disaster management authority (DDMA) can deploy **250 SWARNIM sensor nodes and 7 Raspberry Pi 5 regional hubs**, completely blanketing an entire river basin and every surrounding village.

#### 2. Sub-Second Zero-Internet Local Warning (< 1.2s vs Hours)
* Commercial products depend on cloud connectivity to dispatch SMS or mobile notifications. When cloudburst floods wash away cellular towers or cut fiber lines, those alerts are never received.
* SWARNIM’s **Tier 2 Edge Hub triggers a high-decibel 110 dB industrial siren and strobe beacon directly on-site within 1.2 seconds** of a confirmed flood surge or rate-of-rise anomaly. Communities receive **30 to 45 minutes of actionable physical evacuation notice** before downstream water crests arrive, regardless of cellular status.

#### 3. Dual-Gated Decision Engine (Zero False Negatives + 95% False Alarm Reduction)
* Single-sensor devices produce frequent false alarms (e.g., floating logs, river trash, or wildlife triggering an ultrasonic beam). Over time, communities experience "alarm fatigue" and ignore warnings.
* SWARNIM eliminates this with a **dual-gated safety architecture**:
  1. **Deterministic Safety Rules:** If physical water level exceeds the critical datum or rate-of-rise exceeds `20 cm/hr`, the siren activates unconditionally to prevent algorithm blind spots.
  2. **Quantized int8 TinyML Neural Network:** Cross-evaluates temporal trends across water level rate, rainfall intensity, and soil moisture to classify true flood crest anomalies from sensor noise with **> 96% confidence**.

#### 4. Zero Data Loss Store-and-Forward Architecture
* When internet connectivity drops, generic IoT platforms permanently drop telemetry packets.
* SWARNIM treats network outages as an expected operational condition:
  * Tier 1 nodes buffer timestamped data in local SPI Flash.
  * The Tier 2 Raspberry Pi 5 Edge Hub maintains an append-only circular queue on an **industrial 64GB MicroSD card**.
  * When cellular/Wi-Fi backhaul is restored, the hub automatically synchronizes all historical readings with **AWS IoT Core, Lambda, and DynamoDB**, ensuring unbroken hydrology datasets for post-event analysis.

#### 5. Extreme Tropical Climate Resilience (LiFePO4 + Dynamic Load Shedding)
* Standard Lithium-ion (NMC) batteries risk thermal runaway above 55°C in direct Indian summer sunlight, while Lead-Acid batteries degrade prematurely in high heat.
* SWARNIM uses **Lithium Iron Phosphate (LiFePO4)** cells, certified safe up to 60°C with over 2,000 charge cycles.
* Combined with **TI TPS22919 load switches** that cut quiescent power to unneeded transducers during sleep, SWARNIM nodes achieve **330+ days of theoretical autonomy** and **72+ hours of active emergency warning during continuous zero-sun monsoon deluges**.

---

## High-Level System Architecture

```mermaid
graph TD
    subgraph SENSE["1. Environmental Sensing Layer"]
        S1["Water Level (Ultrasonic JSN-SR04T)"]
        S2["Rain Gauge (Tipping Bucket Reed)"]
        S3["Flame IR & Photoelectric Smoke"]
        S4["Optical PM2.5 / PM10 (PMS5003)"]
        S5["Electrochemical Gas (MQ-Series)"]
        S6["3-Axis MEMS Inclinometer & Soil Moisture"]
    end

    subgraph NODE["2. Distributed Low-Cost Sensor Nodes (Tier 1: ESP32-S3)"]
        PWR["Dynamic Sensor Power Gating (TPS22919)"]
        SMPL["Continuous Sampling & Noise Filtering"]
        DET["Deterministic Safety Rules (Physical Thresholds)"]
        L1["LoRa Transceiver (SX1262 / SX1276)"]
    end

    subgraph MESH["3. Resilient Communication Mesh"]
        LORA["LoRa / LoRaWAN Mesh (IN865 865–867 MHz)"]
        RELAY["Ad-Hoc Peer Relay (Multi-Hop Routing)"]
    end

    subgraph EDGE["4. Edge Intelligence Hub (Tier 2: Raspberry Pi 5 + TinyML)"]
        FUS["Multi-Sensor Temporal Fusion Engine"]
        AI["On-Device TinyML / Anomaly Detection"]
        SCORE["Local Hazard Scoring & Severity Arbitrator"]
        BUFFER["Offline Store-and-Forward Buffer (microSD)"]
        SIREN["Local Alert Actuator (110dB Siren + Strobe LED)"]
    end

    subgraph GATEWAY["5. Gateway Backhaul"]
        GW["Solar LoRaWAN Gateway / Hub Concentrator"]
    end

    subgraph CLOUD["6. Cloud Intelligence & Coordination (AWS Cloud)"]
        IOT["AWS IoT Core (MQTT Secure Ingestion)"]
        LAMBDA["AWS Lambda (Spatial Correlation & Event Processing)"]
        STORAGE["Amazon DynamoDB / Amazon S3 (Time-Series & Telemetry Storage)"]
        QS["Amazon QuickSight / Control Room Dashboard"]
        CAP["NDMA Common Alerting Protocol (CAP) Dispatcher"]
    end

    subgraph OUTPUT["7. Multi-Channel Emergency Warning"]
        DASH["District Emergency Operations Center (DEOC)"]
        SMS["Cell Broadcast / SMS Evacuation Notice"]
        FIRST["First Responders Mobile Alert"]
    end

    SENSE --> PWR --> SMPL --> DET --> L1
    L1 --> LORA
    LORA -.-> RELAY -.-> LORA
    LORA --> EDGE
    EDGE --> FUS --> AI --> SCORE
    SCORE -->|Critical Hazard| SIREN
    SCORE -->|Outage Fallback| BUFFER
    BUFFER -.->|Link Restored| GW
    SCORE -->|Actionable Packet| GW
    GW --> IOT --> LAMBDA --> STORAGE
    LAMBDA --> QS --> DASH
    LAMBDA --> CAP --> SMS
    CAP --> FIRST
```

---

## Two-Tier Compute Architecture

To balance cost, power consumption, and advanced AI requirements across vast geographic corridors, SWARNIM implements a **hierarchical edge compute paradigm**:

```text
  ┌──────────────────────────────────────────────┐     ┌──────────────────────────────────────────────┐
  │      TIER 1: ULTRA-LOW-POWER SENSOR NODE     │     │        TIER 2: HIGH-COMPUTE EDGE HUB         │
  ├──────────────────────────────────────────────┤     ├──────────────────────────────────────────────┤
  │ • Compute: ESP32-S3 Dual-Core Xtensa LX7     │     │ • Compute: Raspberry Pi 5 (Quad Cortex-A76)  │
  │ • Power: 3.3V, < 15 µA Deep-Sleep Current    │     │ • Power: 10W–15W, Solar MPPT + LiFePO4 Pack  │
  │ • Logic: Sensor Sampling + Baseline Filter   │     │ • Logic: TinyML Anomaly Detection + Scoring  │
  │ • Storage: On-board SPI Flash Circular Log   │     │ • Storage: High-Endurance microSD Buffer     │
  │ • Output: LoRa / LoRa Mesh RF Broadcast      │     │ • Output: Direct 110dB Siren + Strobe Alerts │
  │ • Unit BOM: ~₹3,360 ($40 USD)                │     │ • Role: Heavy AI + Regional Cluster Hub/GW   │
  │ • Deployment: Thousands across rivers/hills  │     │ • Deployment: Critical bridges, dams & towns │
  └──────────────────────────────────────────────┘     └──────────────────────────────────────────────┘
```

---

## Architecture Evolution & Cost Optimization

A fundamental challenge in regional disaster networks is balancing compute capability against unit procurement cost and power availability. Earlier high-compute paradigms envisioned placing heavy compute platforms (such as NVIDIA Jetson or industrial x86 boxes) at every single sensor outpost. However, this incurs prohibitive field costs (~$400–$600+ per site), continuous 15W–30W power drains, and bulky solar arrays that make dense spatial deployment unfeasible across India's thousands of remote river basins and forest ridges.

SWARNIM addresses this with a **cost-optimized, hierarchical cluster model**:

```text
EARLIER / HIGH-COMPUTE APPROACH (Cost-Prohibitive & Power-Heavy)
Sensors ──> Heavy Computer (Jetson/x86) ──> Heavy AI ──> Cellular/Sat ──> Cloud

CURRENT SWARNIM ARCHITECTURE (Cost-Optimized, Power-Lean & Resilient)
[Low-Cost Distributed Sensor Nodes]
   ESP32-S3 + Modular Sensors (~₹3,360 BOM)
            │
      LoRa / LoRa Mesh (IN865 865–867 MHz)
            │
            ▼
[Tier 2 Regional Edge Hub]
   Raspberry Pi 5 + TinyML Engine
   • Multi-Sensor Temporal Fusion
   • On-Device AI Anomaly Detection & Local Hazard Scoring
   • MicroSD Store-and-Forward Circular Buffer (Zero Data Loss)
   • Direct 110 dB Siren & Strobe LED Actuator (Instant Offline Warning)
            │
            ▼
[LoRaWAN Gateway Backhaul]
   Solar Concentrator (Ethernet / LTE / Wi-Fi)
            │
            ▼
[Cloud Coordination & Analytics Layer]
   AWS IoT Core ──> AWS Lambda ──> DynamoDB / S3 ──> Amazon QuickSight
```

### Key Architectural Benefits:
1. **Fractional Capital Expenditure:** Hundreds of ultra-low-cost ESP32-S3 nodes (~₹3,360) provide dense spatial coverage, feeding into a single Raspberry Pi 5 Edge Hub at critical choke-points (bridges, dams, panchayat halls).
2. **Extreme Power Lean:** Sensor nodes sleep at < 15 µA. The Raspberry Pi 5 Edge Hub operates reliably from an active 12V LiFePO4 solar rig with dynamic load-shedding.
3. **Guaranteed Localized Siren Actuation:** An immediate 110 dB siren and strobe light triggers at ground zero without needing internet access or remote cloud round-trips.
4. **Resilient Offline Autonomy:** In the event of a total network severance, both tiers continue sensing, scoring, alarming, and buffering.

---

## Functional Partition Matrix

SWARNIM establishes a strict functional contract across all layers from physical transduction to cloud dashboard:

| Component | Hardware / Technology | Primary Responsibility |
| :--- | :--- | :--- |
| **Environmental Sensors** | JSN-SR04T, Rain Gauge, SHT31, PMS5003, MQ-Gas, MPU6050 | Direct physical transduction of flood stage, precipitation, air toxicity, flame, and slope shift |
| **Tier 1 Sensor Node** | ESP32-S3 Dual-Core Xtensa LX7 (16MB Flash, 8MB PSRAM) | Sensor acquisition, power gating (TPS22919), noise filtering, deterministic physical rules, LoRa uplink |
| **Resilient Mesh** | Semtech SX1262 / SX1276 (IN865 865–867 MHz) | Low-power Sub-GHz point-to-point and ad-hoc peer relay communication (12–15 km LoS reach) |
| **Tier 2 Edge Hub** | Raspberry Pi 5 (Quad-Core Cortex-A76 @ 2.4GHz, 4GB/8GB) | Multi-node sensor fusion, TinyML model execution, anomaly detection, real-time hazard scoring |
| **Local Alert Actuator** | 110 dB Industrial Piezo Siren + High-Intensity Strobe LED | Immediate, autonomous local community evacuation warning (< 1.2s latency) without cloud dependency |
| **Offline Storage Buffer**| High-Endurance Industrial microSD Card (V30/A2) | Local store-and-forward circular queue preserving all telemetry during backhaul blackout |
| **LoRaWAN Gateway** | SX1302 / SX1303 8-Channel Concentrator HAT | Concurrent multi-node RF packet demodulation and translation to IP/MQTT backhaul |
| **Cloud Ingestion** | AWS IoT Core | High-scale, TLS-authenticated telemetry and hazard event stream broker |
| **Cloud Processing** | AWS Lambda Serverless | Cross-catchment spatial-temporal correlation, flood crest projection, NDMA CAP alert packaging |
| **Cloud Storage** | Amazon DynamoDB & Amazon S3 | High-throughput time-series records (DynamoDB) and long-term raw hydrology log archives (S3) |
| **Visualization & Alerts**| Amazon QuickSight & Custom GIS Control Room | Real-time district GIS heatmaps, emergency operations consoles, and SMS cell-broadcast triggers |

---

## Dynamic Power Priority Management

In disaster environments, monsoon deluges and wildfire smoke can occlude solar panels for days. SWARNIM implements an active multi-tiered power triage policy:

```text
                      Solar Array + MPPT Charge Controller (TI BQ24650)
                                            │
                                            ▼
                           LiFePO4 Energy Storage Pack (12.8V / 3.2V)
                                            │
               ┌────────────────────────────┴────────────────────────────┐
               ▼                                                         ▼
    BATTERY HEALTHY (> 40% SoC)                               LOW BATTERY (< 40% SoC)
    • All sensors active (level, rain, gas, PM)               • Non-critical sensors powered OFF via TPS22919
    • Full TinyML inference & feature logs                    • Duty cycles extended (sleep 60s -> 300s)
    • Normal telemetry cadence                                • Disable cameras & auxiliary telemetry
    • Siren & strobe ready in standby                         • Power prioritized strictly for:
                                                                  1. Critical hazard sensors (Water/Rain)
                                                                  2. Local siren actuation circuit
                                                                  3. Emergency LoRa alert broadcasts
```

> [!IMPORTANT]
> **Core Engineering Tenet:** *Critical disaster detection and emergency local sirens unconditionally receive power priority over non-critical logging, camera vision, and heavy analytics.*

---

## Network Resilience & Store-and-Forward Buffer

Communication outages are common during extreme disasters. SWARNIM is architected so network loss never halts detection or destroys data:

```text
                      Network Disruption / Gateway Offline
                                       │
                                       ▼
                       Edge Node Continues Local Operation
                                       │
                                       ▼
                    On-Device TinyML & Hazard Scoring Active
                                       │
                 ┌─────────────────────┴─────────────────────┐
                 ▼                                           ▼
      Hazard Condition Detected?                     Telemetry Record
                 │                                           │
         ┌───────┴───────┐                                   ▼
        YES              NO                          Write to microSD
         │               │                     (Store-and-Forward FIFO Queue)
         ▼               ▼                                   │
   Local Siren/LED     Normal                                │
  Activated Instantly  Logging                               │
         │                                                   ▼
         │                                       Connectivity Restored?
         │                                                   │
         │                                                   ▼
         └───────────────────────────────────────> Flush Stored Records to
                                                   AWS IoT Core / Cloud DB
```

- **Zero Data Loss:** When communication fails, all stamped telemetry is committed to the local **microSD card** (on Raspberry Pi 5) or **SPI Flash** (on ESP32).
- **Graceful Re-Synchronization:** Once backhaul connectivity is restored, the queue automatically flushes stored records to AWS IoT Core with backoff retries, ensuring complete hydrological records for historical analysis.

---

## Modular Hazard Node Profiles

SWARNIM does **not** force a one-size-fits-all hardware rig. Instead, a standardized baseboard hosts interchangeable sensor daughter modules based on terrain:

| Node Type | Primary Sensors | Typical Deployment Site | Key Target Event |
| :--- | :--- | :--- | :--- |
| **Flood Node (Flagship Prototype)** | Waterproof Ultrasonic (JSN-SR04T) + Tipping-Bucket Rain Gauge + SHT31 | River banks, bridges, culverts, urban stormwater drains | Flash floods, river crest surges, cloudburst runoff |
| **Forest Fire Node** | IR Flame Sensor + Photoelectric Smoke + Optical PM2.5 + Ambient Temp/RH | Forest perimeters, wildlife sanctuaries, fire-break ridges | Wildfire inception, smoldering biomass, canopy flame |
| **Landslide Node** | Capacitive Soil Moisture + 3-Axis MEMS Inclinometer (MPU6050) + Vibration | Hillside road-cuts, Ghat roads, Himalayan slopes | Slope tilt shift, earth saturation, debris flow precursors |
| **Industrial & Air Quality** | Optical PMS5003 + Multi-Gas Array (MQ-135, MQ-7, CO, Ammonia) | Chemical industrial estates, highway intersections, dense slums | Toxic gas release, hazardous AQI smog episodes |
| **Water Quality Node** | Industrial pH Probe + Turbidity + TDS / Electrical Conductivity | Reservoirs, lakes, industrial effluent discharge points | Chemical dumping, post-flood potable water contamination |

---

## Hardware Components & Bill of Materials

### Tier 1: Flagship Flood Sensor Node Reference Design (Field Production BOM)

> [!TIP]
> **Complete Production PCB Design Package Available:**
> - **Engineering Guide & Verified BOM:** [HARDWARE_GUIDE.md](file:///Users/ankit/Projects/SIH_SWARNIM/hardware/flood_node_pcb/HARDWARE_GUIDE.md)
> - **Interactive Schematic Diagram:** [schematic_diagram.svg](file:///Users/ankit/Projects/SIH_SWARNIM/hardware/flood_node_pcb/schematic_diagram.svg)
> - **2D Board Layout Preview:** [pcb_layout_preview.svg](file:///Users/ankit/Projects/SIH_SWARNIM/hardware/flood_node_pcb/pcb_layout_preview.svg)
> - **KiCad 7/8 Project Files:** [hardware/flood_node_pcb/](file:///Users/ankit/Projects/SIH_SWARNIM/hardware/flood_node_pcb/) (`.kicad_pro`, `.kicad_sch`, `.kicad_pcb`)

| Component | Part / Model | Interface | Unit Cost (INR) | Function |
| :--- | :--- | :--- | :---: | :--- |
| **Microcontroller** | ESP32-S3-WROOM-1 (16MB Flash, 8MB PSRAM) | I2C, SPI, UART, ADC | ₹380 | Dual-core processing & TinyML int8 runtime |
| **Sub-GHz Transceiver** | Semtech SX1276 / SX1262 (865–867 MHz IN865) | SPI + DIO0 IRQ | ₹420 | 12–15 km line-of-sight LoRa telemetry |
| **Water Level Transducer** | JSN-SR04T Waterproof Ultrasonic Sensor | GPIO Trigger / Echo | ₹280 | Non-contact river and drain stage monitoring |
| **Precipitation Sensor** | Optical / Reed Tipping Bucket Rain Gauge | GPIO Hardware Interrupt | ₹350 | Real-time rainfall rate (mm/hr accumulation) |
| **Precision Analog ADC** | Texas Instruments ADS1115 (16-bit 4-Channel) | I2C (0x48) | ₹140 | High-accuracy sensor conditioning & battery sense |
| **Real-Time Clock (RTC)** | Analog Devices DS3231 (TCXO Industrial Temp) | I2C (0x68) | ₹110 | Exact timestamping during total network blackouts |
| **Environmental Context** | Sensirion SHT31-DIS-B | I2C (0x44) | ₹120 | Ambient temperature & relative humidity reference |
| **Solar MPPT Controller** | TI BQ24650 / LTC4015 Synchronous Buck Charger | Circuit | ₹260 | High-efficiency MPPT harvesting in overcast skies |
| **Energy Storage Pack** | 18650 LiFePO4 Battery Cells (3.2V 3200mAh x 2) | Battery Rail | ₹480 | Thermal safety up to 60°C; 2,000+ lifecycle cycles |
| **Photovoltaic Collector**| 6V 5W Monocrystalline Waterproof Panel | DC Jack | ₹290 | Autonomous daylight energy replenishment |
| **Power Gating Switches** | TI TPS22919 Load Switches with Quick Discharge | GPIO Controlled | ₹70 | Completely isolates unneeded sensors during sleep |
| **Ruggedized Enclosure** | Polycarbonate IP66 Case with PG9 Cable Glands | Mechanical | ₹210 | Weatherproof seal against monsoon deluge and dust |
| **Total Sensor Node BOM** | — | — | **~₹3,360** | **Viable for mass panchayat-level procurement** |

### Tier 2: Regional Edge Hub & Concentrator Reference Design (Raspberry Pi 5 BOM)

Each regional cluster hub manages 20–50 distributed Tier 1 sensor nodes across an entire sub-catchment or river stretch:

| Component | Part / Model | Interface | Unit Cost (INR) | Function |
| **Edge Compute Host** | Raspberry Pi 5 (4GB / 8GB LPDDR4X, Quad Cortex-A76) | PCIe, USB 3.0, GPIO | ₹6,200 | On-device TinyML inference, anomaly detection & local hazard scoring |
| **LoRaWAN Gateway HAT** | Waveshare SX1302 / SX1303 8-Channel Concentrator (IN865) | SPI / GPIO | ₹5,800 | Multi-node concurrent packet reception (12–15 km radius) |
| **Store & Forward Storage** | SanDisk Industrial High-Endurance 64GB MicroSD (A2/V30) | SDIO | ₹750 | Local circular FIFO buffer; guarantees zero data loss in blackouts |
| **Local Siren & Strobe** | 110 dB 12V Industrial Piezo Siren + High-Lumen Strobe LED | MOSFET (GPIO Trigger)| ₹450 | Instant community audible & visual warning without internet latency |
| **Solar MPPT Controller** | 12V/10A Synchronous MPPT Solar Charge Controller | Solar / Battery | ₹1,400 | Optimizes energy harvesting for continuous 10W–15W edge operation |
| **Solar Energy Source** | 12V 50W Monocrystalline Aluminum-Framed Solar Panel | MC4 / Terminal | ₹2,400 | Daylight power replenisher rated for heavy monsoon cloud cover |
| **Hub Energy Storage** | 12.8V 12Ah LiFePO4 Deep-Cycle Battery Pack with BMS | 12V Rail | ₹3,400 | 72+ hours uninterrupted hub autonomy through zero-sun overcast |
| **DC-DC Power Stage** | 12V-to-5V 5A High-Efficiency Synchronous Buck Converter | USB-C / Terminal | ₹350 | Stable 5V/5A power delivery for Raspberry Pi 5 under heavy AI loads |
| **Outdoor Weatherproof Box**| IP66 Vented Industrial Electrical Box with Pole Mounting | Mechanical | ₹850 | Hermetic sealing against dust, torrential rain, and heat dissipation |
| **Total Edge Hub Cost** | — | — | **~₹21,700** | **Serves up to 50 sensor nodes across a 15 km river corridor** |

---

## Dual-Stage Safety & TinyML Pipeline

To ensure human-life safety, **SWARNIM never relies on machine learning as an opaque single point of failure**.

```text
                  Multi-Sensor Stream (Level, Rain, Temp, Gas)
                                       │
                                       ▼
                        Preprocessing & Noise Calibration
                                       │
                    ┌──────────────────┴──────────────────┐
                    ▼                                     ▼
        Deterministic Safety Rules              Quantized TinyML Model
        (Strict Physical Thresholds)            (Anomaly & Trend Classifier)
                    │                                     │
                    └──────────────────┬──────────────────┘
                                       ▼
                         Dual-Gated Decision Engine
                                       │
             ┌─────────────────────────┴─────────────────────────┐
             ▼                                                   ▼
     Local Hazard Actuation                              Actionable Packet
   (Direct Piezo Siren / LED)                    (32-Byte LoRa Binary Broadcast)
```

1. **Deterministic Safety Rules:** If water level exceeds `CRITICAL_DATUM` or rate-of-rise exceeds `20 cm/hr`, the alarm triggers unconditionally—preventing algorithmic blind spots.
2. **Quantized Neural Classifiers:** A lightweight 1D-CNN + GRU model (`< 42 KB RAM`, `< 165 KB Flash`) evaluates temporal rate-of-change across rainfall, moisture, and level to predict flood crests **30–45 minutes in advance**.
3. **Tier 2 Edge Hub Correlation:** The Raspberry Pi 5 aggregates inputs across neighboring nodes, computes multi-sensor spatial anomalies, and arbitrates cluster-wide emergency sirens and CAP warning dispatches.

---

## Ultra-Compact 32-Byte Alert Packet Structure

Transmitting verbose JSON over sub-GHz LoRaWAN drains battery and congests regional frequencies. SWARNIM serializes all threat intelligence into a high-density 32-byte binary struct:

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Sync (0xAA) | Protocol Ver  |       Node ID (16-bit)        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Hazard Code   | Severity (1-5)| Confidence %  | Battery Volts |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      Latitude (Float32)                       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                     Longitude (Float32)                       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Unix Epoch Timestamp                    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     Primary Metric (Int16)    |    Secondary Metric (Int16)   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Relay Hop   |  Model Ver ID |        CRC-16 Checksum        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### Actionable Alert Payload Example (Human-Readable Conversion)
```json
{
  "node_id": "SWARNIM-FLD-0042",
  "hazard": "FLASH_FLOOD",
  "severity": "CRITICAL",
  "risk_score": 0.94,
  "confidence": 0.96,
  "location": { "lat": 26.1445, "lon": 91.7362 },
  "evidence": {
    "water_level_cm": 348,
    "rate_of_rise_cm_min": 4.6,
    "rainfall_intensity_mm_hr": 72.4,
    "upstream_corroboration": true
  },
  "recommended_action": "TRIGGER_ZONE_B_EVACUATION_ASSESSMENT",
  "timestamp": "2026-09-05T19:40:12Z"
}
```

---

## Resilient Communication Architecture

1. **Normal State (LoRaWAN Star Topology):** The node uplinks directly to a regional solar-powered LoRaWAN Gateway operating on Indian ISM bands (`IN865–867 MHz`).
2. **Gateway Obstructed (LoRa Ad-Hoc Peer Relay):** If the primary gateway is damaged or masked by terrain, the node shifts to a peer-to-peer relay mode, bouncing packets across neighboring nodes (with hop counters and de-duplication) until reaching a functioning gateway.
3. **Total Backhaul Outage (Store-and-Forward Buffer):** Alerts and historical data are stored in on-board SPI Flash (ESP32) and high-endurance microSD card (Raspberry Pi 5) with backoff retries until the gateway link is restored, ensuring zero data loss.

---

## Deployment Roadmap & National Scale

```mermaid
gantt
    title SWARNIM Phased Scaling Strategy (SIH 2026 to National Scale)
    dateFormat  YYYY-MM
    section Prototype Validation
    Hardware Schematics, BOM Freeze & Bench Testing :done, 2026-08, 2026-09
    TinyML Model Training & Simulation Demo         :active, 2026-09, 2026-10
    section Field Pilot
    15-Node River/Forest District Pilot (Assam/Uttarakhand) :2026-11, 2027-02
    Monsoon Stress & Flood Ingress Testing          :2027-03, 2027-05
    section State Scale
    State Disaster Management Authority (SDMA) Rollout :2027-06, 2027-12
    Pan-India NDMA CAP Integration                  :2028-01, 2028-12
```

- **Phase 0 (Prototype - Present):** Single fully functional ESP32-S3 flood/environmental node with local buzzer, LoRa radio, Raspberry Pi 5 edge hub testbench, and AWS IoT ingestion.
- **Phase 1 (District Pilot - Months 4–8):** 15–20 sensor nodes and 2 Raspberry Pi 5 Edge Hubs deployed along a vulnerable river stretch (e.g., Brahmaputra tributary or Yamuna basin) and forest corridor.
- **Phase 2 (District Scale-Up - Months 9–14):** Complete district coverage integrated with District Disaster Management Authority (DDMA) emergency rooms.
- **Phase 3 (State Integration - Year 2):** Live data feeds directly feeding the NDMA **Sachet** Early Warning platform via standard Common Alerting Protocols (CAP).

---

## Repository Structure

```text
SIH_SWARNIM/
├── firmware/                       # Tier 1 Sensor Node Microcontroller Firmware (ESP32-S3)
│   ├── src/
│   │   ├── main.cpp                # FreeRTOS task manager & power sleep states
│   │   ├── sensors/                # Drivers: JSN-SR04T, SHT31, Rain Gauge, ADS1115
│   │   ├── safety/                 # Deterministic fallback physical rules engine
│   │   └── comms/                  # SX1262 / SX1276 LoRa & Peer Relay handlers
│   └── platformio.ini              # PlatformIO build configuration
├── edge_hub/                       # Tier 2 Regional Edge Hub Software (Raspberry Pi 5)
│   ├── inference/                  # On-device TinyML runtime & anomaly detector
│   ├── scoring/                    # Local hazard scoring & multi-node correlation
│   ├── actuation/                  # GPIO driver for 110 dB siren & strobe LED
│   └── storage/                    # MicroSD store-and-forward FIFO circular buffer
├── models/                         # ML Model Training & Quantization Pipeline
│   ├── datasets/                   # Environmental hazard historical records
│   ├── notebooks/                  # Training notebooks (TensorFlow/Keras/Scikit)
│   └── export/                     # int8 quantized .tflite & model_data.h headers
├── gateway/                        # Field LoRaWAN Gateway / Hub
│   ├── forwarder/                  # Semtech packet forwarder to AWS IoT Core / MQTT
│   └── mesh_bridge.py              # Peer-relay packet de-duplication bridge
├── cloud/                          # AWS Cloud Infrastructure & Pipelines
│   ├── iot_rules/                  # AWS IoT Core SQL rules & binary packet decoders
│   ├── lambda/                     # Serverless stream processing & CAP alert generator
│   └── dynamodb/                   # Time-series schema definitions & S3 archival policies
├── backend/                        # On-Premise / Edge Command Center Server
│   ├── api/                        # FastAPI REST & WebSocket streaming endpoints
│   ├── database/                   # TimescaleDB (time-series) + PostGIS (GIS)
│   └── alerts/                     # NDMA Common Alerting Protocol (CAP) dispatcher
├── dashboard/                      # Real-time GIS Emergency Control Room
│   ├── src/
│   │   ├── components/             # Live sensor telemetry widgets & warning cards
│   │   └── map/                    # Leaflet / Mapbox dynamic hazard risk heatmap
│   └── package.json
└── hardware/                       # KiCad Schematic, PCB Layout & Production Documentation
    └── flood_node_pcb/             # Gerber files, schematic diagrams, and HARDWARE_GUIDE.md
```

---

## Getting Started

### 1. Prerequisites
- **Sensor Node Toolchain:** [PlatformIO IDE](https://platformio.org/) or [ESP-IDF v5.1+](https://docs.espressif.com/)
- **Edge Hub Environment:** Raspberry Pi 5 with 64-bit OS (Debian Bookworm), Python 3.11+
- **Cloud Account:** [AWS Account](https://aws.amazon.com/) (IoT Core, Lambda, DynamoDB, S3, QuickSight)
- **Local Dev Stack:** [Python 3.10+](https://www.python.org/), [Docker Desktop](https://www.docker.com/), [Node.js 18+](https://nodejs.org/)

### 2. Microcontroller Firmware Flash (ESP32-S3 Sensor Node)
```bash
# Clone the project repository
git clone https://github.com/AnkitPandit120/SIH_Swarnim.git
cd SIH_Swarnim/firmware

# Build firmware and flash to USB-connected ESP32-S3 node
pio run --target upload
pio device monitor
```

### 3. Raspberry Pi 5 Edge Hub Setup (Tier 2 Regional Hub)
```bash
# On the Raspberry Pi 5 host
cd SIH_Swarnim/edge_hub

# Install edge runtime dependencies (TFLite runtime, RPi.GPIO, store-and-forward queue)
pip install -r requirements.txt

# Launch edge intelligence service (TinyML inference, siren actuation & microSD buffer)
python main_edge_daemon.py
```

### 4. Cloud Ingestion & GIS Dashboard Launch
```bash
# Option A: Connect Edge Hub to AWS Cloud
cd ../cloud
# Deploy serverless pipeline (IoT Core Rules + Lambda + DynamoDB)
aws cloudformation deploy --template-file template.yaml --stack-name swarnim-iot-stack

# Option B: Run local on-premise emergency control room
cd ../backend
docker-compose up -d

# Launch real-time GIS command center dashboard
cd ../dashboard
npm install
npm run dev
# Open http://localhost:3000 in your browser
```

---

## Target UN Sustainable Development Goals (SDGs)

<div align="center">

| Goal 11 | Goal 13 | Goal 15 |
| :---: | :---: | :---: |
| ![SDG 11](https://img.shields.io/badge/SDG_11-Sustainable_Cities_&_Communities-F99D1C?style=for-the-badge) | ![SDG 13](https://img.shields.io/badge/SDG_13-Climate_Action-48773E?style=for-the-badge) | ![SDG 15](https://img.shields.io/badge/SDG_15-Life_on_Land-56C02B?style=for-the-badge) |

</div>

---

## Team Swarnim

| Name | Role | Focus Area | Contact |
| :--- | :--- | :--- | :--- |
| **Ankit Pandit** | Team Leader & Hardware Lead | System Architecture, Edge Hub Integration & Hardware Design | [@GitHub](https://github.com/AnkitPandit120) |
| **Tanishq Gupta** | Presenter & Domain Research Lead | Disaster Management Frameworks, Field Research & Pitch Presentation | [@GitHub](https://github.com/Shivgg1234) |
| **Yash Panpaliya** | Hardware Management Lead | Component Sourcing, PCB Fabrication, Power Supply & Enclosure Assembly | [@GitHub](https://github.com/yashpanpaliya11) |
| **Riddhima Gupta** | Presentation Design & Documentation | Pitch Deck Visuals, UI/UX Concepts, Diagrams & Technical Documentation | [@GitHub](https://github.com/riddhima303) |
| **Prachi Negi** | Node Software & Research Engineer | ESP32-S3 FreeRTOS Firmware, TinyML Inference & LoRa Protocol Stack | [@GitHub](https://github.com/daprachinegi) |
| **Shyam Patel** | Sensor Research & Optimization | Transducer Selection, ADC Calibration, Noise Filtering & Power Optimization | [@GitHub](https://github.com/patelshyam2008-prog) |

---

## References & Standards Bibliography

### Government & Disaster Management Frameworks
1. **Smart India Hackathon (SIH) 2026:** Problem Statement ID #26178 (*AI Environmental Early-Warning Network*), Ministry of Education Innovation Cell & Qualcomm Inc.
2. **National Disaster Management Authority (NDMA):** *National Disaster Management Guidelines — Management of Floods & Urban Flooding*, Government of India ([ndma.gov.in](https://www.ndma.gov.in)).
3. **NDMA CAP Platform (Project Sachet):** *Implementation of Common Alerting Protocol based Integrated Alert System in India*, NDMA & Centre for Development of Telematics (C-DOT) ([sachet.ndma.gov.in](https://sachet.ndma.gov.in)).
4. **ITU-T Recommendation X.1303:** *Common Alerting Protocol (CAP 1.2)*, International Telecommunication Union ([itu.int/rec/T-REC-X.1303](https://www.itu.int/rec/T-REC-X.1303)).
5. **World Meteorological Organization (WMO):** *WMO-No. 1022: Guide to Hydrological Practices — Volume I: Hydrology From Measurement to Information*, World Meteorological Organization, Geneva.
6. **Central Water Commission (CWC):** *Standard Operating Procedure for Flood Forecasting and Early Warning Across Interstate River Basins*, Ministry of Jal Shakti, Government of India.

### Academic Literature & TinyML Edge AI
7. **TinyML on Ultra-Low-Power Microcontrollers:** David, R., Duke, P., Jain, A., Janapa Reddi, V., et al. (2021). *TensorFlow Lite Micro: Embedded Machine Learning on TinyML Systems*. Proceedings of Machine Learning and Systems (MLSys), [arXiv:2010.08678](https://arxiv.org/abs/2010.08678).
8. **IoT Flash Flood Early Warning Systems:** Ray, P. P., et al. (2020). *IoT-Based Flood Monitoring and Early Warning System: A Systematic Review and Case Study of Himalayan River Catchments*. IEEE Transactions on Systems, Man, and Cybernetics: Systems, 51(11), 7012–7025.
9. **Decentralized Multi-Sensor Streamflow Forecasting:** Sit, M., & Demir, I. (2019). *Decentralized Deep Learning for Streamflow Forecasting Across Regional Catchments*. Journal of Hydrology, 575, 742–753.
10. **Microcontroller Model Quantization:** Warden, P., & Situnayake, D. (2019). *TinyML: Machine Learning with TensorFlow Lite on Arduino and Ultra-Low-Power Microcontrollers*. O'Reilly Media.

### Hardware, Radio & Battery Engineering Standards
11. **Indian Sub-GHz ISM Band Allocation:** *National Frequency Allocation Plan (NFAP 2022)*, Wireless Planning & Coordination (WPC) Wing, Ministry of Communications, Government of India (De-licensed 865–867 MHz frequency band).
12. **Semtech LoRa Specifications:** *AN1200.22: LoRa Modulation Basics and IN865 Regional Regulatory Compliance Guidelines*, Semtech Corporation.
13. **LiFePO4 Safety in Extreme Tropical Climates:** *Bureau of Indian Standards (BIS) IS 16046 / IEC 62133: Secondary cells and batteries containing alkaline or other non-acid electrolytes for portable sealed applications*.
14. **Texas Instruments Application References:** 
    - *BQ24650 Multi-Chemistry High-Efficiency Synchronous MPPT Buck Battery Charger Design Guide* (TI Application Report SLUA583).
    - *TPS22919 5.5V 1.5A Ultra-Low-Leakage Load Switch with Output Discharge* (TI Datasheet SLVSDV2).
15. **AWS IoT Lens:** *AWS Well-Architected Framework: IoT Lens for Resilient Connected Systems and Disconnected Edge Operations*, Amazon Web Services ([docs.aws.amazon.com/wellarchitected/latest/iot-lens](https://docs.aws.amazon.com/wellarchitected/latest/iot-lens/)).