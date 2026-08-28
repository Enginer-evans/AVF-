# AVF-
STM32-based Smart Active Voltage Filter (SAVF) for monitoring and improving AC power quality. The project uses voltage/current sensing, PWM control, power electronics, and LC filtering to detect disturbances and provide stable output. Designed and simulated in Proteus for prototype development.

# ⚡ Closed-Loop Dual-Stage Interleaved Boost Inverter

A high-efficiency, multi-stage power electronics system designed, simulated, and routed completely within **Proteus 9 Professional** and driven by an **Arduino Uno** microcontroller architecture. 

This project demonstrates a unique approach to high-voltage generation by stepping up a low-voltage DC source via a regulated interleaved booster block to create a stable high-voltage DC bus, paving the perfect topological path for pure sine-wave (SPWM) AC inverter inversion.

---

## 🏗️ System Architecture & Engineering Strategy

To ensure absolute simulation stability, minimize current ripple, and prevent SPICE engine convergence errors, the system is split into distinct functional stages:

1. **AC Input Protection & Conditioning:** Features a 220V AC `VSINE` primary source clamped safely by a configured `10D471K` Metal Oxide Varistor (MOV) to suppress line voltage spikes, flowing into a step-down transformer and bridge rectifier.
2. **Active Capacitance Multiplier Filter:** Utilizes a heavy-duty NPN `TIP41C` power transistor network acting as an active voltage filter buffer. This stage smooths out raw rectified ripple voltage down to an ultra-clean **14.4V DC** rail without requiring massive, space-consuming passive capacitors.
3. **Stage 1 Interleaved Boost Converter (14.4V to 60V):** Splitting the power load symmetrically across parallel paths, this stage uses dual `IRF540N` MOSFETs and `100µH` storage inductors. The gates are driven 180° out-of-phase by the Arduino Uno's internal hardware Timer 1 registers at **50kHz**.
4. **Proportional-Integral (PI) Closed-Loop Regulation:** Resolving open-loop voltage drift and control loop oscillations, an active software-defined PI algorithm reads a scaled output rail via a high-headroom `140kΩ / 10kΩ` divider tracking into Analog Pin `A0`. The Arduino dynamically updates the complementary duty cycles to clamp the middle rail rigidly at **60.00V DC**.

---

## 🛠️ Hardware PCB Design Highlights

The logical schematic netlist was translated from scratch into a physical, production-ready printed circuit board (PCB) layout designed to withstand power stresses:

* **High-Voltage Design Rules:** Configured with specific `POWER` net classes mapping thick **`T60` copper trace widths** to maximize current carrying capacity and eliminate trace overheating.
* **Arc Protection Spacing:** Set a hard minimum clearance requirement of **`30th` to `40th` (thou/mils)** to widen the air gap isolation spacing between traces, protecting the board against high-voltage dielectric breakdown or arcing.
* **Double-Layer Optimization:** Routed cleanly utilizing distinct **Top Copper (Red)** and **Bottom Copper (Blue)** layout strategies, fully verified by clearing 100% of Design Rule Checks (DRC) and Connectivity Checks (CRC).
* **3D Visualizer Integration:** Modeled using exact radial and axial part profiles (`DO-41`, `DO-201`, `RAD-0.4`) to generate a complete, populated 3D physical assembly model rendering.

---

## 📂 Repository File Directory

* 📁 `/Hardware` — Primary Proteus 9 schematic captures (`.pdsprj`), routing files, and footprint packages.
* 📁 `/Firmware` — Optimized C++ Arduino closed-loop PI regulation controller sketch script.
* 📁 `/Docs` — Production reference screenshots capturing schematic layouts, circuit net traces, and 3D visualizer board models.

---

## 🚀 How to Run the Simulation

1. Clone or download this repository to your local drive.
2. Open the `.pdsprj` file inside **Proteus 9 Professional**.
3. Compile the Arduino sketch inside the **Arduino IDE** and select *Sketch ➔ Export Compiled Binary*.
4. Double-click the virtual **Arduino Uno** block on the Proteus schematic, click the folder icon next to *Program File*, and load the generated `.ino.hex` file.
5. Hit **Play (▶️)**. Monitor the alternating 50kHz out-of-phase square waves on the virtual oscilloscope, and watch the green output voltmeter climb smoothly and lock seamlessly onto a dead-still **`60.00 Volts DC`**.
