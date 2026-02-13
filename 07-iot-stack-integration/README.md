# IoT Stack Integration (MQTT → Node-RED → InfluxDB → Grafana → ESP32)

This project implements a Docker-based IoT system for collecting, processing, and visualizing sensor data.

Sensor values are published via MQTT, processed in a message pipeline, stored in a time-series database, and displayed on dashboards.  
An ESP32 device subscribes to control topics and performs real-time hardware actions (LED toggle).

All services run on a Raspberry Pi using Docker containers.

## Features

- MQTT-based sensor data collection
- Visual message routing and processing (Node-RED)
- Time-series storage (InfluxDB)
- Real-time dashboards (Grafana)
- ESP32 hardware control via MQTT
- Event-based LED toggle from sensor orientation

## Components

### Raspberry Pi (Docker IoT Stack)

- Mosquitto – MQTT broker
- Node-RED – message processing
- InfluxDB – time-series database
- Grafana – dashboards and visualization
- Portainer – container management

### ESP32

- MQTT client device for hardware control

## Data Flow

### Running containers (Portainer)

![Portainer](figures/portainer-containers.png)

All services run as isolated Docker containers on the Raspberry Pi.

### Data processing (Node-RED)

![Temperature flow](figures/node-red-rp.png)

![RuuviTag flow](figures/node-red-ruuvitag.png)

![LED control flow](figures/node-red-toggle.png)

Node-RED acts as the system logic layer:

- subscribes to MQTT topics
- reformats sensor payloads
- stores measurements to InfluxDB
- publishes control commands to ESP32

### Visualization (Grafana)

![Grafana temperature](figures/grafna-rp.png)

![Grafana ruuvitag](figures/grafna-ruuvitags.png)

InfluxDB stores time-series data which Grafana visualizes in real time.

## ESP32 Firmware (`ruuvitag-led-toggle/`)

- connects to Wi-Fi
- connects to MQTT broker
- subscribes to `esp32/led`
- toggles onboard LED according to received commands

## Setup (Raspberry Pi)

### Install IoTstack (Raspberry Pi)

```bash
curl -fsSL https://raw.githubusercontent.com/SensorsIot/IOTstack/master/install.sh | bash
```

Reboot, then:

```bash
cd ~/IOTstack
./menu
```

Select and build:

- Mosquitto
- Node-RED
- InfluxDB
- Grafana
- Portainer

This generates a `docker-compose.yml` file.

### Start the stack:

```bash
docker-compose up -d
```

### Create InfluxDB database

```bash
docker exec -it influxdb influx
CREATE DATABASE sensor_data
quit
```

### Node-RED setup

Open:

```
http://<raspi-ip>:1880
```

- Import `node-red-flow.json`
- Deploy the flow

### Install Python dependencies on RPi (for RuuviTag listener)

```bash
pip install ruuvitag-sensor paho-mqtt asyncio-paho
```

Run:

```bash
python3 listen-ruuvitags.py -b <raspberry-pi-ip> -t sensors/test
```

### ESP32 firmware

```bash
idf.py build
idf.py flash
idf.py monitor
```

## Usage

1. Start the Docker stack on the Raspberry Pi
2. Import `node-red-flow.json` into Node-RED to restore the processing pipeline
3. Run `listen-ruuvitags.py` on the Raspberry Pi to publish sensor data via MQTT
4. Build and flash the ESP32 firmware (`ruuvitag-led-toggle/`)
5. Monitor data and control logic in Node-RED and Grafana dashboards

## MQTT Topics

- `sensors/test` – sensor data from RuuviTag
- `esp32/led` – LED control commands for ESP32

## Project Files

- `listen-ruuvitags.py` – reads RuuviTag BLE advertisements and publishes sensor data to MQTT
- `node-red-flow.json` – Node-RED flow export used to recreate the full processing pipeline
- `ruuvitag-led-toggle/` – ESP32 firmware (ESP-IDF)
