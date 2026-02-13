# MQTT Servo and Blink Control (ESP32, FreeRTOS)

This project implements an **MQTT-controlled embedded system** using the ESP32 and ESP-IDF to manage a servo motor and an LED blink rate.  
The device connects to Wi-Fi, subscribes to dedicated MQTT topics, and reacts in real time.
FreeRTOS tasks and queues are used to decouple networking logic from hardware control, providing safe and deterministic operation of both the servo and LED.

![MQTT diagram](figures/mqtt-diagram.png)

## Features

- Servo angle and LED blink rate control via MQTT topic
- MQTT publish/subscribe communication over TCP
- MCPWM hardware timer (50 Hz PWM for servo)
- FreeRTOS queues between MQTT callback and control tasks
- Last received value is preserved between updates.

## Structure

```
mqtt/
├── main/
│   ├── mqtt_control_main.c     – Wi-Fi + MQTT + message routing
│   ├── servo.c/h               – servo driver + servo task
│   ├── blink.c/h               – LED blink task
│   └── CMakeLists.txt
├── CMakeLists.txt
└── sdkconfig
```

## How it works

1. ESP32 connects to Wi-Fi and starts an MQTT client.
2. The device subscribes to two topics:
   - servo angle control
   - LED blink rate control
3. Incoming MQTT messages are received inside the MQTT callback and pushed to a FreeRTOS queue.
4. Dedicated low-priority tasks process these queues:
   - **servo task** updates PWM pulse width (angle)
   - **blink task** updates LED delay or toggles using timeout

## Testing

1. Connect the board to Wi-Fi and wait until the device connects to the broker:

   ```
   mqtt://mqtt.eclipseprojects.io
   ```

2. Using any MQTT client (MQTTX, MQTT Explorer, etc.), publish:

   **Servo control** (-90 ... 90)

   ```
   Topic: /esp32c6/servo_angle
   Payload: 45
   ```

   **LED blink control** (milliseconds)

   ```
   Topic: /esp32c6/blink_rate
   Payload: 500
   ```

## Notes

- MQTT callbacks run in the networking context
- Hardware control runs in lower-priority tasks
- FreeRTOS queues prevent blocking the MQTT stack
