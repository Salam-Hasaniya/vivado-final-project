# Lock System Controller  
**Author:** Salam Hasaniya  
**Platform:** CHU FPGA Board · **Language:** C++ (Baremetal) · **IDE:** Vivado / Vitis  

---

## Overview  
The **Lock System Controller** is an FPGA-based security system that integrates **keyboard input**, **joystick controls**, **VGA display**, and **motor control** to simulate a real-world digital lock.  
Users can **set, update, and verify a custom combination** to lock or unlock a stepper motor. The design leverages **real-time SPI, PS/2, and VGA interfaces**, demonstrating a fully integrated embedded control system.

---

## Features  

### Input Control  
- **Joystick (SPI):** Enter combo via directional inputs (Up/Down/Left/Right)  
- **PS/2 Keyboard:** Navigate menu and issue commands  

### Display & Feedback  
- **VGA On-Screen Display (OSD):**  
  - Dynamic menu display  
  - Real-time combo feedback  
  - Lock/unlock status  

### Lock Mechanism  
- **Stepper Motor (via GPO):**  
  - Physically simulates lock/unlock states  
  - Smooth rotation control via single-step driver  

### Combo System  
- Adjustable combo length (4–9)  
- Custom combo creation using joystick  
- Combo persistence for verification  
- Unlock only when user input matches stored combo  

### Menu Options  
| Option | Function |
|:--:|:--|
| **1** | Set combo length and define combination |
| **2** | Update existing combination |
| **3** | Lock or unlock system |

---

## Hardware Requirements  

| Component | Function |
|------------|-----------|
| **CHU Board** | FPGA development platform |
| **PS/2 Keyboard** | Menu input and control |
| **Pmod Joystick (SPI)** | Combination entry |
| **Stepper Motor (via GPO)** | Simulated lock actuator |
| **VGA Monitor** | Display menu and system feedback |

---

## System Architecture  

### 📡 Hardware Interfaces  
- **SPI Bus:** Communicates with Pmod Joystick  
- **PS/2 Interface:** Handles keyboard input events  
- **VGA Controller:** Displays UI through OSDCore and FrameCore  
- **GPIO Core:** Drives stepper motor sequences  

### Software Modules  
| Module | Description |
|:--|:--|
| `get_joystick_direction()` | Reads directional input from joystick via SPI |
| `log_direction()` | Logs joystick direction to UART and buffers it for comparison |
| `lock_motor()` / `unlock_motor()` | Controls stepper motor sequence to simulate lock or unlock |
| `combo_matches()` | Validates entered combo against stored one |
| `read_combo_with_joystick()` | Guides user through combo creation process |
| `display_*()` functions | Manage all OSD/VGA text rendering |
| `update_lock_status()` | Updates status on VGA in real time |

---

## Code Snapshot  

```cpp
#define COMBO_MAX 9
#define COMBO_MIN 4

Ps2Core ps2(get_slot_addr(BRIDGE_BASE, S11_PS2));
SpiCore jstk_spi(get_slot_addr(BRIDGE_BASE, S4_JSTK));
GpoCore motor(get_slot_addr(BRIDGE_BASE, S14_PMOD_GPO));
OsdCore osd(get_sprite_addr(BRIDGE_BASE, V2_OSD));

int combo_len = 4;
char combo[COMBO_MAX] = {'U','D','L','R'};
bool locked = false;
