# FreeRTOS Multitasking Blink (ESP32)

Demonstrates basic multitasking with FreeRTOS on ESP32 using multiple independent LED tasks.

Each LED is controlled by its own task running at a different frequency, showing how tasks share CPU time and execute concurrently.

## Features

- 3 independent blink tasks
  - RED – 3 Hz
  - YELLOW – 7 Hz
  - GREEN – 11 Hz
- GPIO initialization and LED control abstraction
- Optional runtime task statistics logging

## Structure

```
freertos-blink/
├── main/
│   ├── freertos_blink_main.c       – creates tasks
│   ├── led_control.c/.h            – LED GPIO init + blink task implementation
│   └── CMakeLists.txt
├── CMakeLists.txt
└── sdkconfig
```
