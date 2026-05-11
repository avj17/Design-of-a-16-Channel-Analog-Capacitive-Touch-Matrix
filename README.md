# 16-Channel Analog Capacitive Touch Matrix

**A mixed-signal hardware project utilizing Hardware-Parallel Pipelines and a custom 4-Bit Priority Encoder.**

Built by Arun Venkat Jonna and Sangameshwar Sale at IIIT Hyderabad.

---

## 📌 Project Overview
This project demonstrates the seamless integration of low-level analog capacitive physics with high-speed microcontroller routing to create a highly robust 16-channel touch matrix. By mathematically tuning an analog front-end and engineering a novel multiplexing architecture, the system translates microscopic human capacitance ($\approx 25\text{pF}$) into stable digital logic without overwhelming the host microcontroller.

[📺 Watch the Project Demonstration Video Here](https://youtube.com/watch?v=bUG1As17Vn8&feature=shared)

---

## ✨ Key Architectural Novelties

### 1. Hardware-Parallel, Software-Sequential Scanning
To overcome ESP32 pin starvation and high latency, the system utilizes dual $8\times1$ analog multiplexers driven by a shared 3-bit address bus. 
* **The Hardware Wait:** The ESP32 sleeps for a mathematically derived 80ms while dual FVC capacitors charge simultaneously in parallel on the breadboard.
* **The Software Read:** To prevent fatal dual-core ADC memory collisions, the software executes strictly sequential microsecond reads. 
* **Result:** Cuts total system latency by 50% (down to a highly responsive 640ms sweep) while maintaining absolute system stability.

### 2. 4-Bit Hardware Priority Encoder
Replaced a complex, flickering 16-LED display array with a custom 5-pin output structure (4 binary bits + 1 idle indicator).
* Bypasses microcontroller pin starvation.
* Eliminates visual multiplexing flicker.
* **Industrial Priority:** The sequential scanning loop naturally prioritizes the highest active channel during multi-touch events, mimicking industrial "Emergency Stop" safety overrides.

### 3. Mathematically Calibrated Analog Front-End
* **Relaxation Oscillator:** Built around a $\mu$A741 operational amplifier, translating base and human capacitance into frequency shifts.
* **Parasitic Compensation:** Drastically reduced timing resistors ($6\text{M}\Omega \rightarrow 2.2\text{M}\Omega$) to mathematically offset $\approx 31\text{pF}$ of hidden stray capacitance introduced by the CMOS multiplexers.
* **Tunable Digitization:** Utilizes a potentiometer-tuned $1.8\text{V}$ reference comparator with a $26\text{mV}$ hysteresis feedback loop for clean, bounce-free digital triggering.

---

## ⚙️ System Stages

1. **Stage 1 (Analog Front-End):** The physical detection zone. Translates capacitance shifts into frequency shifts, and finally into a stable DC voltage via a diode-pump Frequency-to-Voltage Converter (FVC).
2. **Stage 2 (Multiplexing & MCU):** The routing zone. Funnels 16 sensor signals through dual CD4051 multiplexers into two ESP32 ADC pins.
3. **Stage 3 (Digitization & Output):** The discrete logic zone. Compares the FVC voltage against a $1.8\text{V}$ threshold and outputs the highest active channel to the 4-bit Priority Encoder display.

---

## 🛠️ Hardware Bill of Materials (BOM)
* **Microcontroller:** ESP32 Development Board
* **Op-Amps:** $\mu$A741 General-Purpose Operational Amplifiers (Running on $\pm 6\text{V}$ Rails)
* **Multiplexers:** 2x CD4051 ($8\times1$ Analog MUX/DEMUX)
* **Passive Components:** $10\text{pF}$ Base Capacitors, $2.2\text{M}\Omega$ Timing Resistors, $1\text{M}\Omega$ Hysteresis loops, and various diodes for FVC and ESP32 pin protection.
* **Touch Interface:** 16x Aluminum Foil Sensor Pads
* **Display:** 5x Standard LEDs (4-bit binary + 1 idle state)

---

## 🤝 Contributors
* **Arun Venkat Jonna** (2024102067) - *Core Signal Pipeline, System Timing & Multi-Touch Logic*
* **Sangameshwar Sale** (2024102017) - *Hardware Routing, Safety Clamping & Priority Encoder UI*

**Institution:** ECE Undergraduate Program, IIIT Hyderabad (SARCS Lab)  
**Date:** May 2026
