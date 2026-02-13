# GPIO Blink (ESP32, ESP-IDF)

ESP32 project demonstrating basic GPIO output control using the ESP-IDF framework.

An LED connected to a GPIO pin is toggled periodically to verify:

- toolchain setup
- firmware build/flash workflow
- hardware wiring
- correct GPIO operation

## Features

- GPIO configured as digital output
- periodic LED toggle using FreeRTOS delays
- simple and reusable project template for further experiments

## Structure

```
blink/
├── main/
│   ├── blink.c        – application entry point and GPIO logic
│   └── CMakeLists.txt – component registration
├── CMakeLists.txt     – project build configuration
└── sdkconfig          – ESP-IDF configuration
```
