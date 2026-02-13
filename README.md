# IoT Essentials

<p align="center">
  <img src="https://img.shields.io/badge/ESP--IDF-framework-blue" />
  <img src="https://img.shields.io/badge/FreeRTOS-rtos-purple" />
  <img src="https://img.shields.io/badge/CMake-build-informational" />
  <img src="https://img.shields.io/badge/BLE-gatt-lightblue" />
  <img src="https://img.shields.io/badge/MQTT-protocol-yellow" />
  <img src="https://img.shields.io/badge/Docker-containers-blue" />
  <img src="https://img.shields.io/badge/Node--RED-flow-red" />
  <img src="https://img.shields.io/badge/InfluxDB-timeseries-lightblue" />
  <img src="https://img.shields.io/badge/Grafana-visualization-orange" />
</p>

## Overview

A collection of **embedded and IoT examples built with ESP32 and ESP-IDF**, covering core concepts such as GPIO control, state machines, multitasking with FreeRTOS, interrupt handling, wireless communication (BLE, MQTT), and full IoT stack integration.

Each directory is a self-contained application demonstrating a specific technique — from simple LED blinking to a complete Docker-based sensor-to-dashboard pipeline on Raspberry Pi.

## Projects

- **[GPIO Blink](01-gpio-blink/)** – basic GPIO control
- **[Traffic Light FSM](02-traffic-light-fsm/)** – finite state machine design
- **[FreeRTOS Multitasking](03-freertos-multitasking/)** – concurrent tasks
- **[ISR Latency Measurement](04-isr-latency-rmt/)** – interrupt timing analysis
- **[BLE Servo Control](05-ble-servo-control/)** – BLE + MCPWM motor control
- **[MQTT Servo & Blink](06-mqtt-servo-blink/)** – Wi-Fi + MQTT device control
- **[IoT Stack Integration](07-iot-stack-integration/)** – full Docker IoT pipeline

## Requirements

- ESP-IDF
- CMake
- ESP32 development board
- Raspberry Pi (for IoT stack project)

## Build & Flash (ESP32 projects)

Each project is independent.

```bash
cd <project-folder>
idf.py build flash monitor
```
