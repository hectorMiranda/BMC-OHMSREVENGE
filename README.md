# BMC - Ohm's Revenge

An STM32F103C8 microcontroller project that implements an analog control system with PWM output and directional control.

## 🎯 Project Overview

This project creates a bidirectional motor control system using analog input from a potentiometer. The system reads analog values and converts them to PWM signals with directional control, making it suitable for applications like servo control, motor speed control, or other analog-to-PWM conversion tasks.

## 🔧 Hardware Specifications

- **Microcontroller**: STM32F103C8T6 (Blue Pill)
- **Package**: LQFP48
- **Clock**: 8MHz HSI (Internal oscillator)
- **Development Environment**: STM32CubeIDE
- **HAL Library**: STM32Cube FW_F1 V1.8.6

## 📌 Pin Configuration

| Pin | Function | Description |
|-----|----------|-------------|
| **PA1** | TIM2_CH2 (PWM Output) | PWM signal output for speed control |
| **PA5** | GPIO Output | Direction control pin (Forward) |
| **PA7** | GPIO Output | Direction control pin (Reverse) |
| **PB1** | ADC1_IN9 | Analog input from potentiometer |
| **PA13** | SWDIO | Serial Wire Debug I/O |
| **PA14** | SWCLK | Serial Wire Debug Clock |

## ⚙️ Peripheral Configuration

### ADC (Analog-to-Digital Converter)
- **Instance**: ADC1 Channel 9 (PB1)
- **Mode**: Continuous conversion
- **Resolution**: 12-bit (0-4095)
- **Sampling Time**: 1.5 cycles

### Timer (PWM Generation)
- **Instance**: TIM2 Channel 2 (PA1)
- **Prescaler**: 127
- **Period**: 625
- **PWM Frequency**: ~100Hz

### GPIO
- **PA5, PA7**: Output pins for directional control
- **PA13, PA14**: SWD interface for debugging

## 🚀 Functionality

### Control Logic
The system implements a dead-zone control mechanism:

1. **Neutral Zone** (1790 < ADC < 2220): Motor stopped
   - Both direction pins LOW
   - PWM duty cycle = 0

2. **Reverse Direction** (ADC < 1790): 
   - PA5 = HIGH, PA7 = LOW
   - PWM speed = (1790 - ADC_value) / 3

3. **Forward Direction** (ADC > 2220):
   - PA5 = LOW, PA7 = HIGH  
   - PWM speed = (ADC_value - 2220) / 3

### Key Features
- **Dead Zone**: Prevents unwanted movement around center position
- **Proportional Control**: Speed proportional to analog input deviation
- **Bidirectional**: Forward and reverse operation
- **50ms Update Rate**: Stable control loop timing

## 🛠️ Building and Flashing

### Prerequisites
- STM32CubeIDE or compatible toolchain
- ST-Link programmer or compatible
- ARM GCC toolchain

### Build Instructions
```bash
# Using STM32CubeIDE
1. Open BMC-OHMSREVENGE.ioc in STM32CubeIDE
2. Build Project (Ctrl+B)
3. Flash to target (F11)

# Using Make (if available)
cd Debug/
make clean
make all
```

### Memory Usage
- **Flash**: STM32F103C8 (64KB)
- **RAM**: 20KB
- **Stack**: 1KB
- **Heap**: 512B

## 🔌 Hardware Connections

```
STM32F103C8          External Components
─────────────        ──────────────────
PB1 (ADC) ──────────── Potentiometer wiper
PA1 (PWM) ──────────── Motor driver PWM input
PA5       ──────────── Motor driver DIR1
PA7       ──────────── Motor driver DIR2
3.3V      ──────────── Potentiometer VCC
GND       ──────────── Potentiometer GND
```

## 📊 Performance Characteristics

- **ADC Resolution**: 12-bit (4096 levels)
- **PWM Resolution**: ~625 levels
- **Control Frequency**: 20Hz (50ms loop)
- **Dead Zone**: ±215 ADC counts around center
- **Speed Range**: 0-143 PWM levels per direction

## 🐛 Debugging

The project includes SWD interface for debugging:
- **SWDIO**: PA13
- **SWCLK**: PA14

Use STM32CubeIDE debugger or OpenOCD with ST-Link.

## 📁 Project Structure

```
BMC-OHMSREVENGE/
├── Core/
│   ├── Inc/           # Header files
│   ├── Src/           # Source files
│   └── Startup/       # Startup assembly
├── Drivers/           # HAL drivers
├── Debug/            # Build output
├── BMC-OHMSREVENGE.ioc # STM32CubeMX config
└── README.md         # This file
```

## 🔄 Future Enhancements

- [ ] Add UART communication for parameter tuning
- [ ] Implement PID control for better response
- [ ] Add emergency stop functionality
- [ ] Include position feedback support
- [ ] Add configuration via external EEPROM

## 📝 License

This project is provided as-is under the STMicroelectronics license terms.

## 👨‍💻 Author

Project developed for BMC (Básico de Microcontroladores) coursework.

---
*Generated on July 31, 2025*