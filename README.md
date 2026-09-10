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

## Pure Hardware CMOS Digital Clock

This project is a 100% hardware-based digital timekeeper built strictly from 74HC-series logic gates. It features zero microcontrollers, no firmware, a discrete 240V step-down power supply, and a seamless zero-latency battery failover system. To prevent signal crosstalk on high-speed lines, the physical build exclusively utilizes flush-cut 22 AWG solid-core wire.

### Core Logic & Counter Architecture

The timekeeping chain relies on 74HC390 Dual Decade Counters manipulated into three distinct modulo configurations:

* **Modulo-10 (Seconds & Minutes Ones):** The native 74HC390 state divides by 2 and 5 to naturally count 0 through 9 before rolling over.
* **Modulo-6 (Seconds & Minutes Tens):** Counts 0 through 5. A 74HC08 AND gate detects binary 6 (`0110` via pins `Q_B` and `Q_C`) and fires a hardware reset pulse directly into the `CLR` pin.
* **Modulo-12 (Hours System):** Counts 1 through 12. Dual AND gates detect binary 13 (`0001` in tens, `0011` in ones) to reset the Tens flip-flop back to 0 and pulse-preset the Ones stage back to 1.

### MUX Clock Steering & Precision Setting

To set the time precisely without halting the master oscillator, the clock utilizes combinational multiplexing.

* **Routing Matrix:** A 74HC157 Quad 2-to-1 Multiplexer steers clock pulses. In RUN mode, it passes the live 1Hz tick and carry pulses. In SET mode, it disconnects the carry lines and injects manual tactile switch pulses directly into the isolated unit.
* **Debouncing:** Manual switch inputs pass through an RC low-pass filter (1µF capacitor) into a 74HC14 Schmitt Trigger, snapping the slow analog voltage into a clean digital pulse to prevent double-triggering.
* **Display Logic:** Six 74HC4511 decoders drive Common Cathode displays. Segment current is restricted using 470Ω resistors to maintain a highly efficient sub-80mA total power draw.

### Custom Uninterruptible Power Path

The system runs on a discrete AC-DC grid with priority hardware switching.

* **Mains Step-Down:** A discrete 240V-to-9V AC transformer feeds a DB107 bridge rectifier, smoothed by a 1000µF capacitor, and locked to 5.0V DC via an LM7805 regulator.
* **Battery Failover:** A 3.7V 18650 cell sits in standby, fully managed by an onboard TP4056 charger and an MT3608 boost converter.
* **Diode-OR Priority:** Two 1N5817 Schottky diodes connect both 5.0V sources to the system bus. If mains power drops, the battery diode naturally forwards current with a negligible 0.2V drop, keeping the logic running without resetting the counters.
