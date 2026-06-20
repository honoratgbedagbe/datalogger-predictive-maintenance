# Components of the Intelligent Industrial Datalogger

## 1. STM32F407VET6 Microcontroller

- **Type**: ARM Cortex-M4 32-bit with FPU
- **Frequency**: 168 MHz
- **Memory**: 512 KB Flash, 192 KB RAM
- **Voltage**: 3.3 V
- **Role**: Runs the complete firmware (FreeRTOS, TinyML, Modbus TCP, embedded web server). Handles sensor acquisition, data processing, SD card storage, and Ethernet communication.

## 2. MPU6050 Accelerometer (GY-521)

- **Type**: 3-axis MEMS
- **Interface**: I2C (address 0x68)
- **Range**: ±2 g to ±16 g
- **Resolution**: 16 bits
- **Voltage**: 3.3 V
- **Role**: Measures vibrations on X, Y, Z axes of rotating machinery. The data is used by the TinyML model to detect anomalies (imbalance, misalignment, bearing fault).

## 3. DS18B20 Temperature Sensor

- **Type**: Digital OneWire sensor
- **Range**: -55 °C to +125 °C
- **Accuracy**: ±0.5 °C
- **Resolution**: 12 bits (0.0625 °C)
- **Voltage**: 3.3 V to 5 V
- **Role**: Measures the surface temperature of the machine (pump body, bearing, motor casing). Detects abnormal heating.

## 4. ACS712-30A Current Sensor

- **Type**: Hall-effect sensor
- **Interface**: Analog output (ADC)
- **Range**: ±30 A
- **Sensitivity**: 66 mV/A
- **Supply voltage**: 5 V
- **Role**: Measures the stator current of the motor. Current spectral analysis (MCSA) detects electrical faults (broken rotor bar, eccentricity).

## 5. W5500 Ethernet Module

- **Type**: Hardwired TCP/IP Ethernet controller
- **Interface**: SPI
- **Protocols**: TCP, UDP, IPv4, ICMP, ARP
- **Voltage**: 3.3 V
- **Role**: Provides Modbus TCP communication and the embedded web server. The TCP/IP stack is integrated in hardware, offloading the microcontroller.

## 6. MicroSD Card (SPI Module)

- **Interface**: SPI
- **File system**: FATFS
- **Voltage**: 3.3 V
- **Role**: Stores raw sensor data and the event log (timestamped CSV).

## 7. Pressure Sensor (optional, for pumps and compressors)

- **Type**: 4-20 mA or 0-5 V transmitter
- **Role**: Measures output pressure of a pump or compressor. Cavitation and leaks produce pressure fluctuations.

## 8. Speed Sensor (Hall Effect)

- **Interface**: Pulse (GPIO)
- **Role**: Measures shaft rotation speed. Used for gearboxes (abnormal slip) and to confirm the rotation frequency.

## 9. DC-DC 24 V → 5 V Power Supply

- **Type**: Buck converter (LM2596)
- **Input**: 7-40 V DC
- **Output**: 5 V DC, 3 A
- **Role**: Powers the entire electronics from the standard industrial 24 V supply.

## 10. Safety Relay (SRD-05VDC)

- **Type**: 5 V electromechanical relay
- **Switching capacity**: 250 VAC / 30 VDC, 10 A
- **Role**: Pre-actuator controlled by the microcontroller. Cuts off the motor power supply in case of a critical alert.
