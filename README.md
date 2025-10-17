# Lock System Controller  
**Author:** Salam Hasaniya  
**Platform:** Custom Microcontroller on FPGA (MicroBlaze + Custom HDL Cores)  
**Languages:** Verilog, VHDL, C (Bare-Metal)  
**Tools:** Vivado · Vitis · Nexys A7  

---

## Overview  
The Lock System Controller is a hardware–software co-designed embedded system built entirely on an FPGA platform.  
It operates on a custom microcontroller architecture implemented using the MicroBlaze processor, an FPro bridge for memory-mapped I/O, and user-defined HDL cores for peripheral control.  

This project demonstrates the complete design flow of a microcontroller-based system implemented on FPGA hardware—from HDL core creation, bus integration, and MMIO control, to C-level driver development and embedded firmware.  
The final application simulates a digital lock system that uses joystick and keyboard input, VGA display output, and motor actuation to perform secure lock and unlock operations.

---

## System Features  

### Input and User Interaction  
- **Joystick (SPI):** Directional input (Up, Down, Left, Right) used to enter or modify combination codes.  
- **PS/2 Keyboard:** Menu navigation and numeric control for configuration options.  

### Display and Feedback  
- **VGA On-Screen Display (OSD):** Real-time visual feedback including menus, combination entries, and system status.  

### Lock Mechanism  
- **Stepper Motor (GPO Core):** Simulates mechanical lock and unlock actions through controlled rotation sequences.  

### Combination Logic  
- Adjustable combination length (4–9).  
- Ability to define, update, and verify combinations.  
- Unlock permitted only when the entered sequence matches the stored combination.  

### Menu System  
| Option | Function |
|:--:|:--|
| 1 | Set combination length and define sequence |
| 2 | Set or update existing combination |
| 3 | Lock or unlock the system |

---

## Custom Microcontroller Architecture  

### Hardware Components  
- **MicroBlaze CPU:** Central processing unit responsible for running firmware and managing peripherals.  
- **FPro Bridge (MMIO Interface):** Custom bridge enabling address-based access to user-defined peripheral cores.  
- **Custom HDL Cores:**  
  - **GPO Core:** General-purpose digital output driver for LEDs and the stepper motor.  
  - **GPI Core:** General-purpose input driver for switches and signals.  
  - **Timer Core:** Hardware timer accessible through MMIO registers.  
- **SPI, PS/2, and VGA Controllers:** Integrated modules for peripheral communication.  

### Software Layer  
- Developed bare-metal C drivers corresponding to each HDL core.  
- All cores are accessed via defined memory-mapped base addresses using the bridge.  
- Firmware handles real-time joystick polling, keyboard interrupts, VGA display updates, and motor actuation logic.  

### System Hierarchy  
The FPGA design follows a vanilla system hierarchy:

    top/
    ├── bridge/           # MMIO FPro bridge
    ├── cores/            # Custom peripheral cores (GPO, GPI, Timer, SPI, PS2, VGA)
    ├── microblaze/       # Soft-core processor integration
    ├── main.c            # Application firmware
    └── linker.ld         # Memory mapping configuration


## Hardware Requirements  
- CHU or Nexys A7 FPGA board  
- PS/2 keyboard  
- Pmod joystick (SPI interface)  
- Stepper motor connected to the GPO interface  
- VGA monitor for visual output  

---

## Operation Flow  

1. **Startup:**  
   System initializes peripherals and displays the main menu on VGA.  

2. **Combination Setup:**  
   The user selects Option 1 or 2 to define or modify a combination.  
   Directional input from the joystick is used to specify the pattern.  

3. **Locking:**  
   Selecting Option 3 while unlocked triggers the motor to rotate and sets the system to the locked state.  

4. **Unlocking:**  
   Entering the correct combination through the joystick and selecting Option 3 reverses motor direction and unlocks the system.  

---

## Technical Highlights  
- Designed and verified multiple custom HDL cores integrated through a unified MMIO interface.  
- Developed low-level C drivers to interface directly with hardware registers.  
- Demonstrated full-stack embedded design: hardware definition, driver development, and application logic.  
- Implemented VGA OSD for real-time UI and user feedback.  
- Used the stepper motor as a physical actuator to visualize system state transitions.  

---

## Concepts Demonstrated  
- Microcontroller implementation on FPGA hardware.  
- Memory-mapped I/O communication and driver integration.  
- Mixed hardware/software co-design principles.  
- Peripheral interfacing using SPI, PS/2, VGA, and GPIO.  
- Embedded state machine and timing control for user interaction.  

---

## Related Work  
This Lock System Controller was developed as a standalone application on the custom FPGA microcontroller platform designed in ECE coursework.  
For additional Vivado-based labs and foundational projects, refer to:  
**[FPGA Projects Repository – Salam-Hasaniya/fpga-projects-calpoly](https://github.com/Salam-Hasaniya/fpga-projects-calpoly)**

---

## License  
This project is released under the MIT License.  

