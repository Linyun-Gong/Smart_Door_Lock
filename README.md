# Smart Door Lock System

This project implements a smart door lock system using an ESP32 microcontroller. The system integrates multiple components such as an OLED display, RFID reader, keypad, servo motor, and LED to provide a secure and user-friendly locking mechanism.

## Features

- **RFID Authentication**: Supports RFID cards for unlocking the door.
- **Keypad Authentication**: Allows users to enter a password to unlock the door.
- **OLED Display**: Displays system status, password input, and messages.
- **Servo Motor Control**: Locks and unlocks the door.
- **LED Indicator**: Provides visual feedback for system status.
- **Lockout Mechanism**: Locks the system after multiple failed attempts.
- **Auto-lock**: Automatically locks the door after a set duration.

## Components

### 1. **RFID Reader**
- Uses the RC522 RFID module to read card UIDs.
- Supports adding and verifying authorized UIDs.
- Handles card detection and communication via SPI.

### 2. **Keypad**
- 4x4 matrix keypad for password input.
- Supports password entry, clearing, and confirmation.
- Implements a lockout mechanism after multiple failed attempts.

### 3. **OLED Display**
- Displays system status, password input, and messages.
- Provides visual feedback for user interactions.
- Uses I2C communication for data transfer.

### 4. **Servo Motor**
- Controls the locking and unlocking mechanism.
- Supports calibration and precise control of rotation.
- Uses PWM signals for operation.

### 5. **LED Indicator**
- Provides visual feedback for system status.
- Flashes during system startup, password entry, and errors.

## Detailed Pin Instructions

Below is a suggested wiring guide for connecting the components to the ESP32:

| Component    | Signal    | ESP32 Pin   | Notes                                         |
|--------------|-----------|-------------|-----------------------------------------------|
| **RFID (RC522)** | MOSI      | GPIO 23    | SPI Master Out Slave In                       |
|              | MISO      | GPIO 19    | SPI Master In Slave Out                       |
|              | SCK       | GPIO 18    | SPI Clock                                     |
|              | CS        | GPIO 5     | SPI Chip Select                               |
|              | RST       | GPIO 25    | Reset pin (assigned to avoid I2C conflict)    |
| **Keypad**   | Row 1     | GPIO 32    | Configurable in `config.h`                    |
|              | Row 2     | GPIO 33    |                                               |
|              | Row 3     | GPIO 34    |                                               |
|              | Row 4     | GPIO 35    |                                               |
|              | Col 1     | GPIO 26    |                                               |
|              | Col 2     | GPIO 27    |                                               |
|              | Col 3     | GPIO 14    |                                               |
|              | Col 4     | GPIO 12    |                                               |
| **OLED Display** | SDA      | GPIO 21    | I2C Data                                      |
|              | SCL      | GPIO 22    | I2C Clock                                     |
| **Servo Motor** | PWM     | GPIO 13    | PWM output for controlling the servo          |
| **LED Indicator** | LED     | GPIO 2     | Output for status indication                  |

*Note*: Adjust the pin assignments if needed based on your hardware setup and ESP32 board variations.

## Project Structure

```
├── .devcontainer/            # Development container configuration
├── .vscode/                  # VS Code configuration
├── build/                    # Build artifacts (ignored by Git)
├── main/                     # Main application source code
│   ├── CMakeLists.txt        # CMake configuration for the main component
│   ├── config.h              # Configuration constants and macros
│   ├── keypad.c              # Keypad driver implementation
│   ├── keypad.h              # Keypad driver header
│   ├── led.c                 # LED control implementation
│   ├── led.h                 # LED control header
│   ├── main.c                # Main application logic
│   ├── oled.c                # OLED display driver implementation
│   ├── oled.h                # OLED display driver header
│   ├── rfid.c                # RFID reader driver implementation
│   ├── rfid.h                # RFID reader driver header
│   ├── servo.c               # Servo motor control implementation
│   ├── servo.h               # Servo motor control header
├── CMakeLists.txt            # Top-level CMake configuration
├── sdkconfig                 # ESP-IDF configuration file
└── README.md                 # Project documentation
```

## Getting Started

### Prerequisites
- ESP32 development board.
- RC522 RFID module.
- 4x4 matrix keypad.
- OLED display (SSD1306).
- Continuous rotation servo motor.
- ESP-IDF development environment.

### Setup
1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd smartdoorlock
   ```
2. Configure the ESP-IDF environment:
   ```bash
   . $IDF_PATH/export.sh
   ```
3. Build the project:
   ```bash
   idf.py build
   ```
4. Flash the firmware to the ESP32:
   ```bash
   idf.py flash
   ```
5. Monitor the serial output:
   ```bash
   idf.py monitor
   ```

### Default Password
The default password is `1234`. You can modify it in `main.c`.

## Usage

1. **Unlocking the Door**:
   - Enter the correct password on the keypad and press `#`.
   - Scan an authorized RFID card.
2. **Locking the Door**:
   - The door locks automatically after 5 seconds.
   - You can also manually lock the door using the keypad or RFID.
3. **Lockout Mechanism**:
   - After 3 incorrect password attempts, the system locks for 30 seconds.
4. **OLED Display**:
   - Displays the current status, password input, and messages.
