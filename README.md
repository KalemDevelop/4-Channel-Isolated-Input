# Kalem-4-Channel-Isolated-Input

Isolated 4-channel 12–24 V digital input module for safely interfacing industrial signals to 3.3 V / 5 V microcontrollers and PLCs.

This repository contains hardware design files, firmware examples and documentation for the **Kalem-4-Channel-Isolated-Input** board.

---

## 1. Overview

The **Kalem-4-Channel-Isolated-Input** is a compact module that converts four independent 12–24 V DC field signals into optically isolated, low-side logic outputs.

Each channel:

- Accepts a **12–24 V DC** input referenced to a field ground (`GND_H`)
- Drives an **optocoupler** (PC817 / LTV-817 or equivalent) for galvanic isolation
- Provides an **open-collector low-side output** on the MCU side
- Has a **status LED** on the input (field) side

The logic outputs are referenced to `GND_L` and are pulled to ground when the field input is active, making them easy to interface with 3.3 V or 5 V input pins using a pull-up to `VCCMCU`.

---

## 2. Main Features

- 4 × optically isolated digital inputs for **12–24 V DC**  
- Per-channel optocoupler isolation (typ. 3.75 kVrms)  
- Open-collector low-side outputs (active LOW)  
- Compatible with **3.3 V and 5 V** logic (`VCCMCU`)  
- Per-channel input indication LED on the high-voltage side  
- Separate field ground (`GND_H`) and logic ground (`GND_L`)  
- Optional jumper position to tie grounds when isolation isn’t required  
- Simple connector grouping: inputs on one side, low-side signals on the other  
- Ideal front-end for PLC outputs, sensors, switches, and other industrial signals  

> **Note:** Exact ratings and resistor values are given in the PDF datasheet in the `docs/` folder.

---

## 3. Typical Applications

- Interfacing PLC or relay outputs (12–24 V) to MCUs or SBCs  
- Reading industrial switches, sensors, push-buttons and limit switches  
- Galvanic isolation between noisy field wiring and sensitive electronics  
- DIY PLC / machine I/O input modules  

---

## 4. Hardware Overview

### 4.1 Field-side inputs (high side)

Each input uses a resistor network and an LED to feed the optocoupler LED for 12–24 V operation.

Typical field connections:

- `INPUT1(12/24V)` – Channel 1 input  
- `INPUT2(12/24V)` – Channel 2 input  
- `INPUT3(12/24V)` – Channel 3 input  
- `INPUT4(12/24V)` – Channel 4 input  
- `GND_H` – Common field ground for all four inputs  

Connect your field device between **INPUTx** and **GND_H** (or from a PLC 24 V output to `INPUTx`, with the PLC 0 V tied to `GND_H`).

Each active input lights its LED.

---

### 4.2 MCU-side / low-side outputs

The optocoupler transistors are on the “low side” and act as open-collector outputs referenced to `GND_L`.  

Typical MCU connector signals:

- `VCCMCU` – Logic supply (typically 3.3 V or 5 V)  
- `LOW_SIDE_SIGNAL1` – Channel 1 open-collector output (active LOW)  
- `LOW_SIDE_SIGNAL2` – Channel 2 open-collector output (active LOW)  
- `LOW_SIDE_SIGNAL3` – Channel 3 open-collector output (active LOW)  
- `LOW_SIDE_SIGNAL4` – Channel 4 open-collector output (active LOW)  
- `GND_L` – Logic ground  

> Add pull-up resistors from each `LOW_SIDE_SIGNALx` to `VCCMCU` (either on the board or in firmware using internal pull-ups).

---

### 4.3 Ground isolation

- **Isolated mode** – keep `GND_H` and `GND_L` separate for full galvanic isolation  
- **Non-isolated mode** – fit the optional ground-link jumper so `GND_H` = `GND_L`  

---

## 5. Basic Connection Example

1. Connect the field 24 V system ground to `GND_H`.  
2. Connect PLC / sensor outputs to `INPUT1…INPUT4`.  
3. On MCU side, connect `VCCMCU` (3.3 V or 5 V) and `GND_L` to your controller.  
4. Enable pull-ups to `VCCMCU` on each `LOW_SIDE_SIGNALx` (external resistor or MCU internal pull-ups).  
5. Read `LOW_SIDE_SIGNALx`:
   - Input **active** ⇒ optocoupler saturated ⇒ output **LOW**  
   - Input **inactive** ⇒ transistor off ⇒ pull-up ⇒ output **HIGH**


