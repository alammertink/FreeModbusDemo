# FreeModbus Demo for STM32G4

This project demonstrates the integration of the FreeModbus library with STM32G4 microcontrollers, specifically targeting the Nucleo-G431RB development board. It provides a simple, reusable implementation of a Modbus RTU slave device.

## Project Goals

- Develop a generic STM32 port for the FreeModbus library
- Test implementation on the STM32G4 (Nucleo-G431RB) platform
- Create a simple, reusable demo application showcasing Modbus RTU slave functionality
- Make the library easily integrable into other STM32 projects via CMake
- Focus on portability across different STM32 families
- Provide thorough documentation to help users integrate the library

## Hardware

This project targets the STM32 Nucleo-G431RB development board:

- STM32G431RB microcontroller
- ARM Cortex-M4 core with FPU and DSP instructions
- 170 MHz max CPU frequency
- 128 KB Flash, 32 KB SRAM
- USART2 connected to ST-LINK for virtual COM port
- Arduino-compatible headers for easy peripheral connections

## Getting Started

### Prerequisites

- Visual Studio Code with STM32 VS Code Extension
- STM32CubeMX (for project generation)
- ARM GCC Toolchain
- CMake
- Ninja (optional, for faster builds)

### Building the Project

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/FreeModbusDemo.git
   cd FreeModbusDemo
   ```

2. Initialize and update submodules:
   ```bash
   git submodule init
   git submodule update
   ```

3. Open the project in VS Code and build using the STM32 extension:
   - Press `Ctrl+Shift+P` to open the command palette
   - Type `STM32: Build` and press Enter

4. Flash the device:
   - Press `Ctrl+Shift+P` to open the command palette
   - Type `STM32: Download` and press Enter

## Repository Structure

- `freemodbus/` - The FreeModbus library as a submodule
  - `demo/STM32_CMAKE/` - STM32 port using CMake
- `Inc/`, `Src/` - STM32CubeMX generated code
- `Drivers/` - STM32 HAL drivers
- `cmake/` - Build system configuration

## Usage

The demo application implements a simple Modbus RTU slave device with register functionality. The device responds to standard Modbus requests for reading input registers.

To interact with the device, you'll need a Modbus master device or software tool like Modbus Poll. Connect to the device via UART with the following settings:

- Baud rate: 115200
- Data bits: 8
- Parity: None
- Stop bits: 1
- Flow control: None

## License

This project contains components under different licenses:

- **FreeModbus Core Library**: BSD License - See `freemodbus/modbus/` directory
- **Port Implementation**: LGPL License - Port files are generally licensed under LGPL
- **Demo Application**: GPL License - Demo applications are generally licensed under GPL

This licensing structure follows the original FreeModbus project's licensing approach. Please refer to the individual source files for specific license details, and the license files (`bsd.txt`, `lgpl.txt`, and `gpl.txt`) in the original FreeModbus repository for full license text.

When using this code in your own projects, be aware of the licensing implications, particularly the copyleft requirements of the LGPL and GPL components.

## Acknowledgments

- FreeModbus library by Christian Walter
- STM32 HAL libraries by STMicroelectronics
