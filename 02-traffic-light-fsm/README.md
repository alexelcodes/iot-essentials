# Traffic Light FSM (ESP32, ESP-IDF)

Traffic light controller implemented as finite state machine (FSM) on ESP32.

The project is split into a pure logic module (no GPIO/timing) and a small GPIO driver layer. The main loop advances the internal clock once per second and updates LEDs based on the current state.

## Features

- Day mode (06:00–22:00): RED → RED+YELLOW → GREEN → YELLOW → RED
- Night mode (22:00–06:00): blinking YELLOW
- Logic module independent from GPIO and delays (`traffic_light_logic`)
- Separate GPIO control module (`gpio_lights`)

## Structure

```
traffic-light-system/
├── main/
│   ├── traffic_light_main.c      – init + 1 Hz update loop
│   ├── gpio_lights.c/.h          – GPIO init and LED state output
│   └── traffic_light_logic.c/.h  – FSM + internal time keeping
├── CMakeLists.txt
└── sdkconfig
```
