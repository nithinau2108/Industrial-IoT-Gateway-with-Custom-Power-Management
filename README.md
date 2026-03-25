# Industrial IoT Gateway — 4-Layer PCB Design

A 4-layer industrial-grade IoT gateway PCB that bridges RS485 Modbus 
sensors to a cloud dashboard via WiFi and MQTT. Designed in KiCad 8 
for factory floor deployment with 9V–28V industrial power input.

---

## Overview

Industrial machines speak RS485 Modbus — a wired protocol built for 
noisy factory environments. Cloud dashboards speak WiFi and MQTT. 
This board bridges both worlds: it reads sensor data from RS485 
devices, processes it locally on an ESP32-S3, and publishes live 
readings to an MQTT broker accessible from any browser.

Real-world use cases: factory machine monitoring, agricultural sensor 
networks, building automation, energy metering systems.

---

## Hardware Features

- **MCU** — ESP32-S3-WROOM-1, dual-core 240MHz, WiFi + BLE
- **Power** — TPS54331 buck converter, 9V–28V input → 5V 3A
- **LDO** — TLV1117-33, 5V → 3.3V clean rail for MCU and sensors
- **Industrial comms** — MAX3485 RS485 transceiver, ±15kV ESD 
  protected, half-duplex Modbus RTU
- **USB programming** — CH340C USB-UART bridge with USB-C connector
- **Timekeeping** — DS3231 RTC with I2C interface
- **Local storage** — W25Q64 8MB SPI flash for offline data logging
- **Protection** — TVS diode + polyfuse on power input

---

## Design Decisions

### Why TPS54331 buck converter instead of a linear LDO for 12V→5V?
At 400mA load, a linear regulator stepping 12V to 5V dissipates 
(12-5) × 0.4 = 2.8W as heat — requiring a heatsink and reducing 
reliability. The TPS54331 achieves ~90% efficiency, dissipating 
under 0.3W. No heatsink needed. Critical for a device running 24/7 
in an industrial environment.

### Why RS485 instead of direct WiFi sensors?
Industrial machines installed 10–40 years ago already have RS485 
Modbus interfaces. Replacing them is not feasible. RS485 also 
operates reliably over 1200m of cable in electrically noisy 
environments — impossible for WiFi. The gateway translates between 
the legacy wired world and modern cloud connectivity.

### Why 4-layer PCB instead of 2-layer?
The TPS54331 buck converter switches at 570kHz generating significant 
EMI. A dedicated inner ground plane on Layer 2 contains this noise 
and prevents it coupling into the ESP32-S3 WiFi antenna. A 2-layer 
design would compromise wireless performance. The 4-layer stackup 
also provides a clean power plane on Layer 3, reducing power 
distribution impedance across the board.

### Why MAX3485 specifically?
The MAX3485 operates at 3.3V — direct compatibility with ESP32-S3 
GPIO without level shifting. It includes ±15kV ESD protection 
essential for factory environments and supports up to 32 nodes on 
a single RS485 bus.

---

## PCB Specifications

| Parameter | Value |
|---|---|
| Layer count | 4 |
| Stackup | Signal / GND / PWR / Signal |
| Board dimensions | 100mm × 80mm |
| Min trace width | 0.2mm (signal), 0.5mm (power) |
| Min clearance | 0.2mm |
| Surface finish | HASL lead-free |
| Manufactured by | JLCPCB |

---

## Repository Structure
```
industrial-iot-gateway-v1/
├── schematic/
│   ├── industrial-iot-gateway.kicad_sch
│   └── industrial-iot-gateway-schematic.pdf
├── pcb/
│   ├── industrial-iot-gateway.kicad_pcb
│   └── screenshots/
│       ├── top-copper.png
│       ├── bottom-copper.png
│       └── 3d-view.png
├── gerber/
│   └── industrial-iot-gateway-gerbers.zip
├── bom/
│   └── bom.csv
└── README.md
```

---

## Project Status

- [x] Datasheets studied — TPS54331, MAX3485, ESP32-S3
- [x] Power tree designed
- [ ] Schematic complete
- [ ] ERC passing — zero errors
- [ ] PCB layout complete
- [ ] DRC passing — zero errors
- [ ] Gerber files exported
- [ ] BOM finalised with LCSC part numbers
- [ ] Design review document written

---

## Tools Used

- **KiCad 8** — schematic and PCB layout
- **JLCPCB** — PCB manufacturing (design rules applied)
- **LCSC** — component sourcing and BOM pricing
- **HiveMQ** — MQTT broker for cloud dashboard

---

## Key Datasheets

- [TPS54331](https://www.ti.com/product/TPS54331) — Buck converter
- [MAX3485](https://www.analog.com/en/products/max3485.html) 
  — RS485 transceiver
- [ESP32-S3](https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf) 
  — Main MCU

---

## Author

Nithin A U 
B.E. Electrical and Electronics  Engineering  
R N S Institute of Technology, [2025-26]  
Linkedin - www.linkedin.com/in/nithin-a-u-912199385 
Email id - aunithin2108@gmail.com
