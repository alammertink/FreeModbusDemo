# FreeModbusDemo Project Guidelines

## Project Goals

- Develop a generic STM32 port for the FreeModbus library
- Test implementation on the STM32G4 (Nucleo-G431RB) platform
- Create a simple, reusable demo application showcasing Modbus RTU slave functionality
- Make the library easily integrable into other STM32 projects via CMake
- Focus on portability across different STM32 families
- Provide thorough documentation to help users integrate the library

## Repository Structure

- The `freemodbus` submodule contains the main FreeModbus library we're working with
  - This includes a port for the ADUC7XXX series in `freemodbus/demo/ADUC7XXX/`
  - Our new STM32 generic port is in `freemodbus/demo/STM32_CMAKE/`
  - The STM32_CMAKE directory already contains port files (port.h, portevent.c, portserial.c, porttimer.c)
- The `libfreemodbus` directory contains an STM32CubeIDE project with a port for the STM32F1xx series
  - The STM32F1xx port files are located in `libfreemodbus/modbus/modbus_port/`
  - While we're using the company's existing library in the freemodbus submodule, this STM32F1xx port is useful for reference and inspiration
- The FreeModbusDemo repository itself contains platform-specific files (currently for STM32G4)
  - This separation of platform-specific code from the library allows better organization
  - By using VSCode + CMake (instead of STM32CubeIDE), we can maintain this separation cleanly

## CubeMX Generated Files

Files like main.c, system_stm32xxx.c and stm32xxx_it.c are coupled to the MXCube
configuration tool. When these files contain comment blocks like:

  /* USER CODE BEGIN XXXX */

  /* USER CODE END XXX */

these are markers where new code can be added without CubeMX touching it. Always 
use these sections when modifying CubeMX-generated files.

## HAL Driver

- Use low-level LL driver where appropriate for performance-critical functionality
- STM32 HAL library is used for general peripheral configuration
- Ensure USE_FULL_LL_DRIVER preprocessor directive is set when needed

## Code Development Style
- Aim for readability and maintainability as the primary goals
- Prefer descriptive variable names over excessive comments, but do include purposeful comments to explain things that aren't immediately obvious, like manipulating registers or explaining calculations. 
- Maintain 80 column width where reasonable
- Avoid including header files that are not needed, minimize dependencies
- Align variable declarations, method declarations, etc. vertically for improved readability
- Avoid using the heap
- Use "tight parentheses" style:
  - Control keywords should NOT have a space between the keyword and opening parenthesis:
    `if( condition )` rather than `if (condition)`
  - Function calls, control keywords and definitions should have spaces inside the parentheses:
    `function( arg1, arg2 )` rather than `function(arg1, arg2)`
- Use Allman style indentation (braces on new lines) except for:
  - "Early return"/"guard clauses" which can be kept on one line
  - Functions/methods that fit entirely on one line
  - Example:
    ```c
    // Regular function with Allman style
    void function(void)
    {
      // body
    }
    
    // Early return example
    int getValue(bool condition)
    {
      if(!condition) return defaultValue;
      
      // rest of function
    }
        
    // One-line function
    static inline int max(int a, int b) { return a > b ? a : b; }
    ``` 
    
- Use vertical spacing to group related code and emphasize important sections
- Include simple getters/setters and short one-line functions in header files to keep implementation files cleaner

## Hardware Description

This project targets the STM32 Nucleo-G431RB development board:

- STM32G431RB microcontroller
- ARM Cortex-M4 core with FPU and DSP instructions
- 170 MHz max CPU frequency
- 128 KB Flash, 32 KB SRAM
- USART2 connected to ST-LINK for virtual COM port
- Arduino-compatible headers for easy peripheral connections

## Project Memory

### Key Architectural Decisions
- FreeModbus library ported as a static library for STM32 platform
- Generic port architecture with conditional compilation for different STM32 families
- UART communication with interrupt-driven approach for efficient operation
- Hardware timer for accurate Modbus protocol timing
- Simple CMake integration for easy inclusion in other projects
- Focus on Modbus RTU mode as primary protocol variant

### Implementation Status
- Basic STM32 port structure created
- Port files implemented:
  - port.h: Platform-specific type definitions
  - portevent.c: Event handling mechanism 
  - portserial.c: UART communication (needs UART initialization implementation)
  - porttimer.c: Timer handling for protocol timeouts
- Demo application scaffolding in place
- Build system configured with CMake
- Currently compiles for STM32G4, but not functional yet due to missing UART initialization

### Development Roadmap
1. **Complete STM32_CMAKE Port** (HIGH PRIORITY)
   - Implement UART initialization in portserial.c
   - Ensure proper timer configuration
   - Test basic communication

2. **Implement Demo Application**
   - Refine `demo.c` to work with the Nucleo-G431RB
   - Implement register callbacks (input, holding, coils)
   - Setup basic Modbus slave functionality
   - Use the `demo/BARE` directory as reference implementation

3. **Test on Nucleo-G431RB Platform**
   - Verify UART communication
   - Test timer accuracy
   - Validate Modbus protocol compliance
   - Test with commercial Modbus master tools

4. **Documentation and Examples**
   - Create comprehensive integration guide
   - Document API usage
   - Provide examples for STM32G4 family

### Memory Management Guidelines
- Update this Project Memory section after each significant development milestone
- Record key architectural decisions in the appropriate subsection
- Track implementation status changes as components are completed
- Add new subsections as needed for different aspects of the project
- Include dates with major updates to maintain a project timeline

### Technical Roadblocks
- Problems encountered and their solutions
  - [Add specific problems and solutions as they arise]

### Testing Goals
- Verify correct operation on the G4 platform
- Test Modbus RTU protocol compliance with standard tools (e.g., Modbus Poll)
- Validate register read/write operations
- Ensure proper error handling and response to invalid requests
- Measure performance and resource usage

### Future Enhancements
- Consider adding ASCII mode support with thorough testing
- Explore Modbus master functionality
- Implement additional examples for various STM32 families
- Create detailed integration guide for common use cases
