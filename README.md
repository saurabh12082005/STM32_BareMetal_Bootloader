STM32 Bare-Metal Bootloader
🚀 Project Overview
This repository contains the source code for a complete bare-metal bootloader and application system for an ARM Cortex-M4 (STM32F401VE) microcontroller. The project is built entirely from scratch without using any vendor HAL or LL libraries, focusing on direct register access to control the hardware.

The system demonstrates a two-stage boot process where a permanent bootloader, residing in a protected region of Flash memory, validates and then jumps to a separate user application. The entire workflow is verified through a simulation in Proteus using a 16x2 character LCD for visual feedback.

✨ Key Features
Bare-Metal C: All code is written in pure C with direct memory-mapped register access. No vendor libraries were used.

Custom Bootloader: A bootloader developed from scratch that performs a safe and robust handoff to the main application.

Custom Linker Scripts: Manually authored linker scripts (.ld) to precisely partition the microcontroller's Flash memory into separate, non-overlapping regions for the bootloader and the application.

Robust Handoff Mechanism: The bootloader de-initializes all its peripherals and resets the system clock to a safe state before jumping, providing a clean environment for the application to start.

Complete Simulation: The entire system is designed and validated within a Proteus simulation, proving the concept without needing physical hardware.

🛠️ System Architecture
The firmware is divided into two distinct parts within the STM32's 512KB of Flash memory.

+--------------------------------+  <-- 0x08000000 (Flash Start)
|                                |
|   Bootloader (32 KB)           |
|   (Permanent, runs on reset)   |
|                                |
+--------------------------------+  <-- 0x08008000
|                                |
|   Application (480 KB)         |
|   (Updatable, runs after jump) |
|                                |
|                                |
+--------------------------------+  <-- 0x0807FFFF (Flash End)
On power-on, the CPU begins execution at 0x08000000, starting the Bootloader.

The bootloader initializes the LCD and displays its status message.

After a 3-second delay, the bootloader cleans up system resources and jumps to the application's entry point at 0x08008000.

The Application runs, initializes the LCD for its own use, and displays its status message.

⚙️ Tech Stack
Language: C

Target MCU: STM32F401VE (ARM Cortex-M4)

IDE: STM32CubeIDE

Build Toolchain: GCC for ARM

Simulation: Proteus Design Suite

Firmware Merging: srec_cat or Python (intelhex library)

📋 How to Build and Run
Create Projects: In STM32CubeIDE, create two separate projects for the STM32F401VE:

M4_Bootloader

M4_RTOS_App (or a similar name for your application)

Add Files: For each project, copy the corresponding main.c and linker script (.ld) file provided.

Build Both: Build both projects to generate two .hex files.

Merge HEX Files: Use srec_cat or a similar tool to merge the two files into a single combined.hex file.

Bash

srec_cat M4_Bootloader.hex -intel M4_RTOS_App.hex -intel -o combined.hex -intel
Set up Proteus: Create the schematic with the STM32F401VE and the LM016L LCD, wiring the control and data pins as specified in the code (PB0/PB1 for control, PC0-PC7 for data). Ensure the LCD's power pins (VDD, VSS, VEE) are connected.

Load Firmware: Double-click the STM32 component in Proteus and load the final combined.hex file.

Run Simulation: Start the simulation.

🎬 Simulation Output
When the simulation is run, the LCD will display a two-stage output, confirming the entire process was successful.

First, the LCD will show:

Bootloader...
After 3 seconds, the second line will be added:

Bootloader...
Application!
(Here you should add a screenshot or GIF of your working Proteus simulation!)

[INSERT_SIMULATION_GIF_HERE]
