### Pure CMOS Digital Logic Clock

> A 100% hardware-based digital clock built entirely from standard 74HC-series logic gates. No microcontrollers, no code—just pure combinational and sequential digital logic.

This project demonstrates how to build a highly reliable, glitch-free timekeeping system featuring discrete modulo counters, precision multiplexed time-setting, and an uninterruptible power supply (UPS) built from basic components.

---

### System Architecture

* **Timekeeping Stages:** Utilizes 74HC390 Dual Decade Counters cascaded into specialized hardware logic: Mod-10 (0-9) for standard digits, Mod-6 (0-5) for tens of seconds/minutes via 74HC08 AND gates, and Mod-12 (1-12) for hours.
* **Glitch-Free Multiplexing:** Implements a 74HC157 Quad 2-to-1 MUX to safely steer clock lines. It allows individual isolation of Hours, Minutes, or Seconds for precise manual time-setting via tactile switches without interrupting the master 1Hz timebase.
* **Hardware Debouncing:** Uses RC low-pass networks fed into 74HC14 Schmitt Triggers to eliminate mechanical switch bounce, ensuring perfectly clean single-step incrementing.
* **Display Logic:** Drives 0.56-inch Common Cathode 7-segment displays using 74HC4511 BCD-to-7-Segment decoders with optimized 470-ohm current-limiting resistors for low power consumption.

---

### Custom Power & Battery Backup

* **Discrete Mains Supply:** Drops 240V AC to 9V AC using a PCB-mount transformer, rectified to DC via a full-wave bridge, and regulated to a clean 5V logic rail using an LM7805.
* **Seamless Battery Failover:** An integrated 18650 Li-ion cell (managed by a TP4056 charger and MT3608 boost converter) sits in standby.
* **Zero-Reset Technology:** Two 1N5817 Schottky diodes form a hardware Power-OR gate. If mains power drops, the battery instantly takes the load. The logic state never drops, preventing the counters from resetting to zero.

---

### Core Bill of Materials (BOM)

| Component | Part Number | Primary Function |
| --- | --- | --- |
| **Counters** | 74HC390 | Base sequential counting logic |
| **Decoders** | 74HC4511 | BCD to 7-segment translation |
| **Multiplexer** | 74HC157 | Clock routing (Run/Set modes) |
| **Logic Gates** | 74HC08 / 74HC14 | Modulo resets and signal squaring |
| **Power Switch** | 1N5817 | Schottky diode-OR backup failover |

* **Wiring Philosophy:** To prevent signal cross-talk and maintain a clean, semi-permanent breadboard/perfboard layout, this project strictly uses flush-cut 22 AWG solid-core copper wire instead of standard stranded jumper cables.
