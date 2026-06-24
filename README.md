# Buck Converter — TPS40200 (12V to 5V, 5A)

A synchronous-capable asynchronous buck converter designed using the **TPS40200** wide-input PWM controller IC from Texas Instruments. The board steps down 12V to a regulated 5V output capable of delivering up to 5A continuous current.

---

## Specifications

| Parameter | Value |
|-----------|-------|
| Input Voltage | 12V DC |
| Output Voltage | 5V DC |
| Max Output Current | 5A |
| Switching Frequency | ~200kHz |
| Inductor | 10µH |
| Output Capacitance | 100µF + 47µF (parallel) |
| Current Sense Resistor | 17mΩ (R7) |
| Controller IC | TPS40200 (Texas Instruments) |

---

## Tools Used

| Tool | Purpose |
|------|---------|
| KiCad 7 | Schematic capture and PCB layout |
| TINA-TI / LTSpice | Circuit simulation |
| TPS40200 (TI) | PWM controller IC |

---

## Circuit Overview

The design uses the **TPS40200** as the PWM controller driving an external **N-channel MOSFET** (Q1) in a non-synchronous buck topology with a **Schottky diode** (D1) for freewheeling.

**Key functional blocks:**

- **PWM Controller (U1 — TPS40200):** Generates the switching signal via internal oscillator set by RC network on the RC pin (R9 = 160kΩ, C8 = 1.9nF)
- **Power Switch (Q1 — N-MOSFET):** Switches the input voltage at the switching node
- **Freewheeling Diode (D1 — Schottky):** Conducts during the MOSFET off-time, completing the inductor current path
- **LC Filter (L1 = 10µH, C9 = 100µF + C3 = 47µF):** Smooths the switching waveform into a clean 5V DC output
- **Current Sensing (R7 = 17mΩ):** Shunt resistor on the high-side for overcurrent protection via ISNS pin
- **Feedback Network (R1 = 100kΩ, R2 = 10kΩ, R3 = 62kΩ):** Sets the output voltage regulation point via the FB pin
- **Compensation Network (COMP pin):** Type-II compensation for loop stability

---

## Schematic Preview

![Schematic](docs/schematic.png)

---

## PCB Layout Preview

![PCB Layout](docs/pcb_layout.png)

---

## 3D Render

![3D Render](docs/3d_render.png)

---

## DRC Results

| Check | Result |
|-------|--------|
| Violations | 0 ✅ |
| Unconnected Items | 0 ✅ |
| Ignored Warnings | 5 (courtyard, footprint filters — non-critical) |

---

## Design Decisions

**Why TPS40200?**
The TPS40200 is a flexible wide-input non-synchronous PWM controller that allows full external component selection — MOSFET, diode, inductor, and compensation network — giving complete control over switching frequency, current limit, and loop dynamics. This makes it ideal for learning power supply design from first principles compared to integrated solutions.

**Why external MOSFET + Schottky?**
Using a discrete N-channel MOSFET and Schottky diode allows independent optimization of switching losses and conduction losses. The Schottky's low forward voltage drop (~0.3V) minimizes freewheeling losses at 5A output.

**Output capacitor selection:**
Two capacitors in parallel (100µF + 47µF) reduce effective ESR, improving transient response and reducing output voltage ripple at full load.

**Current sensing:**
A 17mΩ shunt on the high-side provides accurate current sensing to the ISNS pin for overcurrent protection without significant power loss.

---

## File Structure

```
Buck-Converter-TPS40200/
├── schematics/          # KiCad schematic source + PDF export
├── pcb/                 # KiCad PCB file + Gerber files (ZIP)
├── simulation/          # LTSpice/TINA-TI simulation files
├── docs/                # Screenshots, 3D renders, BOM
│   ├── schematic.png
│   ├── pcb_layout.png
│   └── 3d_render.png
└── README.md
```

---

## How to Open

1. Install [KiCad 7](https://www.kicad.org/download/) or later
2. Clone or download this repository
3. Open the `.kicad_pro` file in KiCad

---

## Author

**Vineeth S Shastry**
B.E. Electronics and Communication Engineering — ATME College of Engineering (2026)
Embedded Systems Intern — L&T Technology Services (LTTS), Mysuru

[![GitHub](https://img.shields.io/badge/GitHub-easy--vin--78-181717?logo=github)](https://github.com/easy-vin-78)
