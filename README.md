# Custom-Bootloader-Development-for-STM32

## STM32 UART Bootloader with CRC Validation

A custom bootloader for STM32 microcontroller (STM32L476RG) built using the STM32 HAL library. This project enables reliable firmware updates over UART with integrity verification using CRC and a structured firmware image format.

**📌 Features**
- 🚀 UART-based firmware update mechanism
- 🧠 Custom flash memory partitioning via linker script
- 🔒 CRC-based firmware integrity verification
- 🧾 Structured firmware header (size + CRC)
- 🔄 Safe application jump after validation
- ⚡ Flash erase and reprogramming using HAL drivers
- 🛡️ Protection against corrupted firmware updates

## Architecture Overview
> The system is divided into two main regions in flash:  
>
> | Region      | Description                    |
> |-------------|--------------------------------|
> | Bootloader  | Handles firmware updates       |
> | Application | User application firmware      |

## Memory Layout

To support safe firmware updates, the internal flash memory of the microcontroller is divided into separate regions for the bootloader, application header, and application firmware.

<img width="323" height="417" alt="memory layout" src="https://github.com/user-attachments/assets/d5c88e39-82ea-44b8-8401-447e5edf854a" />

- **Bootloader Region**
  - Occupies the initial portion of flash memory
  - Remains unchanged during firmware updates
  - Ensures the update mechanism remains operational even if an update fails

- **Application Header**
  - Located immediately after the bootloader
  - Stores metadata such as:
    - Magic value
    - Firmware size
    - CRC checksum (for integrity verification)

- **Application Firmware Region**
  - Contains the main application code
  - Erased and rewritten during firmware updates
  - Receives new firmware image from the host

## Bootloader Workflow

<img width="1222" height="620" alt="c6051493d35cba6128ec41af255874245807ae22" src="https://github.com/user-attachments/assets/e4f10a67-6630-4a0d-a03d-0d21e6904d4e" />

1. **Startup**
   - MCU boots into bootloader
   - Checks for update trigger (e.g., UART activity or flag)

2. **Firmware Reception**
   - Receives firmware over UART
   - Parses firmware header

3. **Validation**
   - Extracts:
     - Firmware size
     - CRC checksum
   - Verifies received data integrity

4. **Flash Operations**
   - Erases application flash sectors
   - Writes new firmware using HAL flash APIs

5. **Verification**
   - Computes CRC of written firmware
   - Compares with expected CRC

6. **Jump to Application**
   - If valid → jumps to application start address
   - If invalid → stays in bootloader


## Future Improvements
- ⬆️ Support for OTA (Over-The-Air) updates
- 🔐 Firmware encryption / authentication
- 📡 Support for CAN / USB DFU
- 🧾 Versioning and rollback mechanism
