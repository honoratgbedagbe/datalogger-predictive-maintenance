# Intelligent Industrial Datalogger for Predictive Maintenance

**Polyvalent embedded platform for rotating machinery monitoring with TinyML**

## Objective

Predictive maintenance for industrial rotating machinery (pumps, motors, compressors, fans, gearboxes) by detecting vibration, thermal and electrical anomalies before they cause costly downtime.

## Key Features

- **Multi-physics acquisition**: 3-axis vibration (MEMS accelerometer), surface temperature, motor stator current.
- **On-board AI (TinyML)**: Autoencoder trained on normal data, running locally on the microcontroller.
- **Granular fault diagnosis**: Identifies the fault type (imbalance, misalignment, bearing defect, cavitation, electrical fault) and the likely faulty component.
- **Remaining Useful Life (RUL)**: Estimates the time before failure with a confidence interval.
- **Automatic protection**: Emergency stop or load shedding when critical thresholds are reached.
- **Industrial communication**: Modbus TCP via Ethernet, embedded cognitive web server.
- **Local storage**: SD card with timestamped event log and raw data in CSV format.
- **Reconfigurable**: TinyML model can be changed for different machines without hardware modifications.

## Hardware

| Component | Role | Interface |
|:---|:---|:---|
| STM32F407VET6 (Cortex-M4, 168 MHz) | Main microcontroller, TinyML inference | — |
| MPU6050 (GY-521) | 3-axis accelerometer for vibration | I2C |
| DS18B20 | Surface temperature sensor | OneWire |
| ACS712-30A | Stator current sensor | ADC |
| W5500 | Ethernet module (Modbus TCP, web server) | SPI |
| microSD card (SPI module) | Data and log storage | SPI |
| DC-DC 24V→5V converter | Power supply from industrial voltage | — |
| Safety relay | Motor cut-off on critical alert | GPIO |

## Software Architecture

- **RTOS**: FreeRTOS (CMSIS_V2)
- **File system**: FATFS
- **TCP/IP stack**: Integrated in W5500 (WIZnet driver)
- **Industrial protocol**: Modbus TCP
- **On-board AI**: TensorFlow Lite for Microcontrollers
- **Embedded HMI**: Web server (HTML/CSS)
- **Languages**: C (firmware), Python (model training, analysis scripts)

## Repository Structure
