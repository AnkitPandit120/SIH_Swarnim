# 🛠️ Flagship Flood / Water-Level Node — Hardware Engineering Guide

**Project:** SWARNIM (Smart Warning & Resilient Network for Intelligence & Monitoring)  
**SIH 2026 Problem Statement ID:** #26178  
**Organization:** Qualcomm Inc.  
**Category:** Hardware (IoT Transduction, Embedded Systems & Edge AI)  
**Board Model:** `SWARNIM-FLOOD-NODE-V1` (Tier 1 Low-Cost Distributed Sensor Node)  
**Revision:** `v1.0-RC1`

---

## 1. System Overview & Hierarchical Architecture

The **SWARNIM Flood Node** is a specialized, solar-autonomous field sensing unit engineered for mass deployment across flood-prone riverbanks, mountain streams, and urban stormwater canals.

Operating as a **Tier 1 Low-Cost Sensor Node** in SWARNIM's hierarchical architecture, this board couples an **ESP32-S3-WROOM-1** edge processor with a **Semtech SX1276 / SX1262** sub-GHz LoRa radio, a high-efficiency **TI BQ24650 MPPT solar charge controller**, a thermally resilient **LiFePO4 battery pack**, and ruggedized environmental transducers (**JSN-SR04T waterproof ultrasonic** and **tipping-bucket rain gauge**).

### Role in the Network
- **Local Transduction & Gating:** Performs precision distance and rainfall sampling while cutting power to unused sensors via high-side **TI TPS22919 load switches**.
- **Deterministic Physical Safety:** Executes instant hardware threshold rules (e.g. rate-of-rise > 20 cm/hr) independently of network availability.
- **Resilient LoRa Uplink:** Broadcasts high-density 32-byte binary telemetry packets over **Sub-GHz LoRa (IN865 865–867 MHz)** to neighboring peer nodes and the **Tier 2 Raspberry Pi 5 Regional Edge Hub**.
- **Zero Data Loss:** If RF connectivity is lost, samples are logged to internal SPI Flash circular buffers and re-transmitted once the link is re-established.

### Visual Diagrams
- **Schematic Architecture:** [`schematic_diagram.svg`](file:///Users/ankit/Projects/SIH_SWARNIM/hardware/flood_node_pcb/schematic_diagram.svg)
- **Top-Down PCB Layout:** [`pcb_layout_preview.svg`](file:///Users/ankit/Projects/SIH_SWARNIM/hardware/flood_node_pcb/pcb_layout_preview.svg)
- **KiCad 7/8 Project:** [`EIN_Flood_Node.kicad_pro`](file:///Users/ankit/Projects/SIH_SWARNIM/hardware/flood_node_pcb/EIN_Flood_Node.kicad_pro)
- **KiCad Schematic:** [`EIN_Flood_Node.kicad_sch`](file:///Users/ankit/Projects/SIH_SWARNIM/hardware/flood_node_pcb/EIN_Flood_Node.kicad_sch)
- **KiCad 4-Layer PCB:** [`EIN_Flood_Node.kicad_pcb`](file:///Users/ankit/Projects/SIH_SWARNIM/hardware/flood_node_pcb/EIN_Flood_Node.kicad_pcb)

---

## 2. Complete ESP32-S3 GPIO Mapping Table

Firmware developers must use the following hardware pin allocations:

| Signal Name | ESP32-S3 Pin | Peripheral / Function | Electrical Specs | Description / Circuit Notes |
| :--- | :--- | :--- | :--- | :--- |
| **`3V3`** | Pin 2 | Power Input | 3.3V DC (600mA max) | Fed by AP2112K LDO regulator; decoupled with 10µF + 0.1µF |
| **`GND`** | Pin 1, 41 (EP) | Power Ground | 0V Common Return | Connected to Layer 2 continuous GND copper plane |
| **`EN`** | Pin 3 | Chip Enable / Reset | Active Low | Pulled up with 10kΩ to 3V3 + 100nF capacitor to GND + SW1 button |
| **`IO0`** | Pin 0 | Boot Mode Select | Active Low | Pulled up with 10kΩ to 3V3 + SW2 button (pull low on boot to flash) |
| **`RAIN_IRQ`** | **IO4** | GPIO External Interrupt | 3.3V Logic (Internal Pullup) | Tipping bucket reed switch input; TVS D2 clamped + 100nF RC debounce |
| **`JSN_TRIG`** | **IO5** | Digital Output | 3.3V Logic Output | 10µs ultrasonic start pulse to JSN-SR04T |
| **`JSN_ECHO_DIV`**| **IO6** | Digital Input | 3.3V Logic Input | **5V Echo divided down to 3.0V** via 2.2kΩ / 3.3kΩ resistor divider |
| **`SENSOR_PWR_EN`**| **IO7** | Digital Output | Active High | Controls TPS22919 load switch to cut 5V sensor power during sleep |
| **`I2C_SDA`** | **IO8** | I2C Data Bus | 3.3V Open-Drain | Shared between SHT31 (0x44), DS3231 (0x68), ADS1115 (0x48) |
| **`I2C_SCL`** | **IO9** | I2C Clock Bus | 3.3V Open-Drain | 4.7kΩ pull-up resistors to 3V3 rail |
| **`LORA_NSS`** | **IO10** | SPI Chip Select | Active Low Output | Dedicated chip select for SX1276 LoRa transceiver |
| **`SPI_MOSI`** | **IO11** | SPI Master Out | 3.3V High Speed | Shared SPI bus to LoRa and MicroSD |
| **`SPI_SCK`** | **IO12** | SPI Clock | Up to 10 MHz | Master clock for LoRa communications |
| **`SPI_MISO`** | **IO13** | SPI Master In | 3.3V Logic Input | Transceiver data return line |
| **`LORA_RST`** | **IO14** | Digital Output | Active Low | Hardware reset for SX1276 module |
| **`LORA_DIO0`** | **IO21** | GPIO External Interrupt | Active High Input | LoRa Packet Receive (RxDone) / Transmit (TxDone) interrupt |
| **`UART0_TXD`** | **IO43** | Serial Transmit | 3.3V TTL | USB-C UART bridge console output (115200 baud) |
| **`UART0_RXD`** | **IO44** | Serial Receive | 3.3V TTL | Firmware programming & interactive terminal |

---

## 3. Verified Bill of Materials (BOM)

All components are standard off-the-shelf parts available from **LCSC, DigiKey, and Mouser**:

| Item | Qty | Reference | Description / Part Number | Footprint / Package | Est. Cost (INR) | Supplier / Source |
| :--- | :---: | :--- | :--- | :--- | :---: | :--- |
| **1** | 1 | **U1** | ESP32-S3-WROOM-1-N16R8 (16MB Flash, 8MB PSRAM) | Module SMD-41 | ₹380 | LCSC C2913200 / Espressif |
| **2** | 1 | **U2** | Semtech SX1276 LoRa Transceiver (865–867 MHz) | SMD-16 (Ra-02 / Ai-Thinker) | ₹420 | LCSC C90039 / Semtech |
| **3** | 1 | **U6** | TI BQ24650 Synchronous MPPT Solar Charger | VQFN-16 (3.5x3.5mm) | ₹260 | DigiKey 296-27138-1-ND |
| **4** | 1 | **U7** | Diodes Inc. AP2112K-3.3TRG1 600mA Low-Dropout LDO | SOT-23-5 | ₹35 | LCSC C37367 / Diodes Inc. |
| **5** | 1 | **U8** | TI TPS61023 Synchronous Boost Converter (5V output) | SOT-563 / SOT-23-6 | ₹45 | LCSC C499981 / TI |
| **6** | 1 | **U9** | TI TPS22919 High-Side Load Switch with Output Discharge | SOT-23-6 | ₹30 | LCSC C499974 / TI |
| **7** | 1 | **U3** | Sensirion SHT31-DIS-B Temp & Relative Humidity Sensor | DFN-8 (2.5x2.5mm) | ₹120 | LCSC C78652 / Sensirion |
| **8** | 1 | **U4** | Analog Devices DS3231SN Real-Time Clock with TCXO | SOIC-16W | ₹110 | LCSC C24855 / ADI |
| **9** | 1 | **U5** | TI ADS1115IDGSR 16-Bit 4-Ch Precision ADC | MSOP-10 | ₹140 | LCSC C37583 / TI |
| **10** | 1 | **L1** | 10µH Shielded Power Inductor (3A Saturation Current) | SMD 6.8x6.8mm | ₹25 | LCSC C167440 / Wurth |
| **11** | 1 | **D1** | SMAJ18A Unidirectional TVS Diode (18V Standoff) | DO-214AC (SMA) | ₹15 | LCSC C382343 / Littelfuse |
| **12** | 1 | **D2** | ESD5Z3.3T1G Bidirectional TVS Diode (3.3V Standoff) | SOD-523 | ₹8 | LCSC C80744 / Onsemi |
| **13** | 2 | **J1, J2** | 2-Pin 5.08mm Pitch Heavy-Duty Screw Terminal Blocks | Through-Hole 5.08mm | ₹40 | LCSC C8457 / Degson |
| **14** | 2 | **J3, J5** | 4-Pin 3.81mm Pitch Pluggable Screw Terminal Blocks | Through-Hole 3.81mm | ₹50 | LCSC C8460 / Degson |
| **15** | 1 | **J4** | 2-Pin 3.81mm Pitch Screw Terminal Block | Through-Hole 3.81mm | ₹20 | LCSC C8458 / Degson |
| **16** | 1 | **J6** | SMA Edge-Mount Bulkhead RF Receptacle (50 Ohm) | Edge Mount SMA-K | ₹65 | LCSC C14848 / Amphenol |
| **17** | 1 | **J7** | USB-C 16-Pin Receptacle (USB 2.0 Flashing Port) | SMD Type-C 31-M-12 | ₹25 | LCSC C165948 / Korean Hro |
| **18** | 1 | **BAT1** | CR1220 Coin Cell SMD Battery Clip (for DS3231 RTC) | SMD 12mm Holder | ₹18 | LCSC C70377 / Keystone |
| **19** | 1 | **JSN-SR04T**| Waterproof Ultrasonic Transducer Module with 2.5m cable | External Sensor | ₹280 | Electronic Comp India |
| **20** | 1 | **RAIN-GAUGE**| Tipping-Bucket Rain Gauge (0.2794 mm/pulse) | External Sensor | ₹350 | Misol / SparkFun |
| **21** | 1 | **SOLAR-5W** | 6V 5W Monocrystalline Waterproof Aluminum Panel | External Hardware | ₹290 | Loom Solar / Waaree |
| **22** | 2 | **18650-LiFe**| 3.2V 3200mAh LiFePO4 Rechargeable Cells | Battery Rail | ₹480 | Li-Ion Motors / EVE |
| **23** | 1 | **BOX-IP66** | Polycarbonate Weatherproof Enclosure (160x110x90mm) | Mechanical Enclosure | ₹210 | CamdenBoss / Generic IP66 |
| **24** | Misc | Passives | 0603 Capacitors, Resistors, LEDs, Pushbuttons, Glands | SMD 0603 | ₹90 | Generic SMD Assortment |
| **TOTAL** | — | — | **Complete Field-Ready Flood Node BOM** | — | **~₹3,366** | **Low-Cost Mass Deployment** |

---

## 4. Power Budget & Battery Autonomy Analysis

### Operating State Power Consumption

| State | MCU Status | Transceiver Status | Sensors Status | Duration | Current Draw (@ 3.3V) |
| :--- | :--- | :--- | :--- | :---: | :---: |
| **Deep Sleep** | ULP Timer Active | SX1276 Sleep Mode | All Sensors Powered Down (TPS22919 OFF) | 58.5 seconds | **14.2 µA** |
| **Sensor Sampling** | Active (240MHz) | Standby Mode | 5V Boost ON + Ultrasonic Ping + Rain IRQ | 600 ms | **48.0 mA** |
| **TinyML Inference** | Dual Core Active | Standby Mode | Sensor Power Cut; TFLite Micro evaluating | 120 ms | **68.5 mA** |
| **LoRa Uplink (TX)** | Active | +20 dBm Output | Inactive | 250 ms | **125.0 mA** |

### Daily Energy Calculation (Normal Mode - 60s Cycle)

- **Sleep Energy:** $14.2\,\mu\text{A} \times \frac{58.5}{60} \times 24\,\text{h} \approx 0.33\,\text{mAh/day}$
- **Sampling Energy:** $48\,\text{mA} \times \frac{0.6}{60} \times 24\,\text{h} \approx 11.52\,\text{mAh/day}$
- **Inference Energy:** $68.5\,\text{mA} \times \frac{0.12}{60} \times 24\,\text{h} \approx 3.29\,\text{mAh/day}$
- **LoRa TX (every 10 min):** $125\,\text{mA} \times \frac{0.25}{600} \times 24\,\text{h} \approx 1.25\,\text{mAh/day}$
- **Total Daily Energy Consumption:** $\approx \mathbf{16.39\,\text{mAh/day}}$ (or $\sim 54\,\text{mWh/day}$).

### Zero-Sunlight Battery Autonomy & Dynamic Power Priority Management

With a **3.2V 6400mAh LiFePO4 battery pack** (2x 3200mAh in parallel):
$$\text{Autonomy Days} = \frac{6400\,\text{mAh} \times 0.85\,\text{efficiency}}{16.39\,\text{mAh/day}} \approx \mathbf{331\text{ Days of Continuous Operation!}}$$

Even during a severe monsoon event with **continuous warning polling (every 5 seconds)**, the pack sustains **72+ hours of complete autonomy without daylight**.

#### ⚡ Active Load Shedding Policy
The firmware monitors battery state of charge (SoC) via the on-board TI ADS1115 ADC:
1. **Normal Operation (SoC > 40%):** Standard 60-second sampling cycle, full ultrasonic burst, rain accumulation tracking, ambient temp/RH readings, and periodic health heartbeats.
2. **Low-Battery Reserve Mode (SoC ≤ 40%):** 
   - Non-critical sensor rails are completely isolated by pulling `SENSOR_PWR_EN` (GPIO 7) low, shutting off the 5V boost converter and auxiliary sensors via the **TI TPS22919 load switch**.
   - Sleep intervals expand to 300 seconds.
   - Power is strictly conserved for **rain gauge interrupt pulse counting**, **critical flood stage threshold trip**, and **emergency LoRa uplink broadcasts**.

---

## 5. PCB Fabrication & Manufacturing Rules

| Parameter | Specification | Manufacturing Rule |
| :--- | :--- | :--- |
| **Layer Count** | **4 Layers** | Stackup: L1 (Signal/RF) - L2 (GND) - L3 (PWR) - L4 (Signal/GND) |
| **Board Dimensions** | **100.0 mm × 80.0 mm** | Tolerance: $\pm 0.1\,\text{mm}$; 4x M3 corner holes at (5mm, 5mm) |
| **Dielectric Material** | FR-4 Standard (Tg 155°C) | Total finished board thickness: $1.6\,\text{mm}$ |
| **Copper Thickness** | $1\,\text{oz}$ ($35\,\mu\text{m}$) Outer & Inner | Solder mask: Dark Forest Green / Matte Black; White Silkscreen |
| **Min Trace / Spacing** | $0.15\,\text{mm} / 0.15\,\text{mm}$ ($6\,\text{mil}$) | Power traces: $0.6\,\text{mm}$ ($24\,\text{mil}$) width |
| **RF Coplanar Waveguide**| **$50\,\Omega \pm 10\%$ Impedance** | Trace width $0.5\,\text{mm}$ with $0.25\,\text{mm}$ GND ground coplanar clearance |
| **Surface Finish** | **ENIG (Electroless Nickel Immersion Gold)** | Crucial for corrosion prevention in high-humidity monsoon terrain |

---

## 6. Field Installation & Maintenance

```text
                  [5W Monocrystalline Solar Panel]
                  (Oriented South at 30° Incline)
                                 │
                                 ▼
                     ┌───────────────────────┐
                     │   IP66 Weatherproof   │
                     │    Polycarbonate      │
                     │    Enclosure Box      │
                     │ (Contains SWARNIM PCB │
                     │  & LiFePO4 Battery)   │
                     └───────────┬───────────┘
                                 │
                   ┌─────────────┴─────────────┐
                   ▼                           ▼
       [Tipping-Bucket Rain Gauge]   [JSN-SR04T Waterproof Transducer]
      (Clear sky, level mounting)   (Mounted on cantilever bracket
                                     perpendicular above river stage)
```

1. **Ultrasonic Placement:** Must be rigidly mounted on a galvanized steel arm protruding over the water body, at least **30 cm above highest historical flood line** (JSN-SR04T blind zone is 20 cm).
2. **Rain Gauge Leveling:** Mount on top of the pole using the integrated spirit bubble level; verify no overhanging tree leaves obstruct rainfall entry.
3. **RF Antenna Orientation:** Mount the external fiberglass omnidirectional antenna vertically for maximum 360° coverage along the river valley.
4. **Site Datum Calibration:**
   * Measure the vertical distance $H_{\text{ref}}$ from the transducer face to the river bed.
   * Write $H_{\text{ref}}$ to the board configuration via USB-C terminal:
     ```bash
     swarnim-config --set-datum 4.500 --node-id SWARNIM-FLD-0042
     ```
   * Live water level is automatically calculated as:
     $$\text{Water Level} = H_{\text{ref}} - D_{\text{measured}}$$

---

## 7. Factory Bring-Up & Test Procedure

Follow this checklist before shipping each production node:

- [ ] **Visual Inspection:** Verify no solder bridging on ESP32-S3 pins, BQ24650 QFN pads, or SHT31 sensor.
- [ ] **Unpowered Impedance Check:** 
  - Resistance between `3V3` and `GND` must exceed $> 50\,\text{k}\Omega$.
  - Resistance between `VBAT` and `GND` must exceed $> 100\,\text{k}\Omega$.
- [ ] **Power Rail Verification:** Connect bench power supply (6V current-limited to 200mA) to Solar In `J1`. Measure test point `TP_3V3`: must read $3.30\,\text{V} \pm 0.05\,\text{V}$.
- [ ] **Firmware Flashing:** Connect USB-C cable to `J7`. Flash `firmware/` via PlatformIO:
  ```bash
  pio run --target upload
  ```
- [ ] **Sensor Loopback Test:** In serial monitor, observe:
  - `[OK] SHT31 detected at 0x44 (Temp: 27.4°C, RH: 64%)`
  - `[OK] DS3231 detected at 0x68 (RTC Clock synced)`
  - `[OK] ADS1115 detected at 0x48 (Battery: 3.32V)`
  - `[OK] JSN-SR04T Ping: 124.5 cm`
  - `[OK] SX1276 LoRa Initialized @ 866.5 MHz (RSSI: -42 dBm)`

---

## 8. Tier 2 Regional Edge Hub (Raspberry Pi 5) Hardware Architecture

To coordinate clusters of 20–50 Tier 1 sensor nodes across a sub-catchment, the **Tier 2 Regional Edge Hub** provides localized AI inference, store-and-forward buffering, and community alarm actuation.

```text
                                  +12V Solar MPPT Bus
                                          │
            ┌─────────────────────────────┼─────────────────────────────┐
            ▼                             ▼                             ▼
   [TI TPS54531 12V➔5V 5A]       [12V 110dB Siren & Strobe]    [12.8V 12Ah LiFePO4 BMS]
            │                             ▲
            ▼                             │ MOSFET (IRLZ44N)
     Raspberry Pi 5                       │
   (Quad Cortex-A76) ───[GPIO 18 / PWM]───┘
            │
            ├── SPI ──> Waveshare SX1302 / SX1303 LoRaWAN Gateway HAT (IN865)
            │
            ├── SDIO ──> 64GB Industrial MicroSD (Store-and-Forward FIFO Queue)
            │
            └── Ethernet / LTE Modem ──> AWS IoT Core / Command Center
```

### Key Hub Hardware Specifications:
1. **Single Board Computer:** Raspberry Pi 5 (4GB or 8GB LPDDR4X) running Debian Bookworm 64-bit with active fan cooler.
2. **LoRaWAN Concentrator HAT:** Waveshare SX1302 / SX1303 with GPS sync, operating on Indian ISM band **IN865 (865–867 MHz)** for multi-channel concurrent demodulation across 20–50 field nodes.
3. **Local Alarm Actuation Stage:**
   - **Actuator:** 12V 110 dB Industrial Piezo Siren + High-Intensity Red LED Strobe.
   - **Driver Circuit:** Logic-level N-channel MOSFET (**IRLZ44N** or **AO3400**) switched via RPi GPIO 18, with 1N4007 flyback diode protection across inductive siren terminals and 10 kΩ gate pull-down resistor.
4. **Store-and-Forward MicroSD Buffer:**
   - SanDisk Industrial 64GB A2/V30 MicroSD card formatted with ext4 journaled filesystem.
   - Holds up to 6 months of raw high-frequency multi-node telemetry in an append-only circular queue during total cellular/satellite outages.
5. **Power Subsystem:**
   - 12V 50W Monocrystalline solar panel + 10A MPPT charge controller.
   - 12.8V 12Ah LiFePO4 deep-cycle battery pack with integrated hardware Battery Management System (BMS).
   - High-efficiency synchronous buck converter delivering continuous 5.1V @ 5.0A with transient headroom for peak AI compute loads.
