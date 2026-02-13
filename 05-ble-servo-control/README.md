# BLE Servo Control (ESP32, FreeRTOS, MCPWM)

The project demonstrates a **BLE-controlled servo motor system** using the ESP32 and ESP-IDF. It integrates BLE communication, FreeRTOS multitasking, and MCPWM-based PWM generation to control servo position remotely.

## Features

- Servo angle control via BLE GATT characteristic write
- MCPWM hardware timer (50 Hz PWM signal generation)
- Reusable `servo.c/.h` driver module
- FreeRTOS queue between BLE stack callback and control task
- Clear separation between BLE communication and hardware control logic

## Structure

```
05-ble-servo-control/
├── gatt-server-service-table/   – BLE GATT server + servo integration
└── mcpwm-servo-control/         – standalone servo PWM test project
```

### gatt-server-service-table

BLE application:

- based on ESP-IDF GATT server example
- receives angle value from BLE write
- sends value to FreeRTOS queue
- dedicated task updates servo position

### mcpwm-servo-control

Standalone PWM test application:

- verifies servo wiring
- validates timing
- extracts reusable driver

## Testing

1. Flash the BLE application (`gatt-server-service-table`)

2. Connect to the device using any BLE client application (e.g. LightBlue, BLE Scanner).

3. Find device:
   ESP_GATTS_XXXX

4. Write an ASCII angle value to the writable characteristic:
   45
   -30
   90

## Notes

- BLE callbacks run inside the BLE stack task
- Servo control runs in a lower-priority task
- Queue is used to avoid blocking BLE stack
- Angle range: −90° … +90°
