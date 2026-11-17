# 🌿 Project EDEM: Classroom Climate Control

[![MicroPython](https://img.shields.io/badge/MicroPython-ESP32-blue.svg)](https://micropython.org/)
[![MQTT](https://img.shields.io/badge/MQTT-Mosquitto-green.svg)](https://mosquitto.org/)
[![SQLite](https://img.shields.io/badge/Database-SQLite-blue.svg)](https://www.sqlite.org/)

**Project EDEM** is an IoT system designed for monitoring microclimate conditions in educational classrooms. The project consists of ESP32-based hardware for data collection and a backend server for receiving, storing, and relaying telemetry data.

---

## ✨ Key Features

*   **Hardware Monitoring:** Reads temperature and humidity (DHT22) and CO2 levels (MH-Z19B).
*   **On-Site Visualization:** Displays current readings and air quality status on a TFT display (ST7735).
*   **Indication:** LED alarms for exceeding temperature and CO2 thresholds.
*   **Wireless Transmission:** Sends telemetry via Wi-Fi using the MQTT protocol.
*   **Server Processing:** A Python backend (Paho MQTT) receives data, saves historical records to an SQLite database, and optionally relays data to a secure MQTT broker.
*   **Fault Tolerance:** Automatically reconnects to Wi-Fi and the MQTT broker upon connection loss.

---

## 🛠 Architecture & Tech Stack

### 1. Hardware & Firmware
*   **Microcontroller:** ESP32
*   **Language:** MicroPython
*   **Sensors:** DHT22 (temperature, humidity), MH-Z19B (carbon dioxide)
*   **Peripherals:** ST7735 TFT display, LEDs
*   **Repository Files:**
    *   `main.py` — Main loop for polling sensors, rendering to the display, and publishing data via MQTT.
    *   `dht.py`, `mhz19.py` — Sensor drivers.
    *   `st7735.py`, `xglcd_font.py`, `font_converted.c` — Display driver and font handling (with Cyrillic support).

### 2. Backend Server
*   **Language:** Python
*   **Database:** SQLite (`sqlite3`)
*   **Protocol:** MQTT (`paho-mqtt`)
*   **Repository Files:**
    *   `init_db.py` — Script to initialize the SQLite database table.
    *   `mqtt_to_sqlite.py` — MQTT client that subscribes to the topic, parses JSON, and saves data to the DB.

---

## 🔌 Pinout & Wiring

All connections are specified for the ESP32 board. Ensure the CO2 sensor is powered by 5V, while the display and temperature sensor require 3.3V.

| Component | Device Pin | ESP32 Pin | Note / Code Function |
| :--- | :--- | :--- | :--- |
| **Display (ST7735 TFT 1.8")** | `VCC` | **3.3V** | Display power |
| | `GND` | **GND** | Common ground |
| | `CS` | **GPIO 5** | Chip Select |
| | `RESET` | **GPIO 4** | Reset |
| | `A0 / DC` | **GPIO 2** | Data / Command |
| | `SDA / MOSI` | **GPIO 23** | Hardware SPI |
| | `SCK` | **GPIO 18** | Hardware SPI |
| | `LED` (Backlight) | **GPIO 21** | Turned on via software (logic 1) |
| **DHT22 Sensor** (Temp/Hum) | `VCC` | **3.3V** | Sensor power |
| | `GND` | **GND** | Common ground |
| | `DATA` | **GPIO 22** | Data bus |
| **MH-Z19B Sensor** (CO2)| `VIN` | **5V / VIN**| Strictly 5V (VIN pin on board) |
| | `GND` | **GND** | Common ground |
| | `TX` | **GPIO 16** | Connects to hardware RX2 |
| | `RX` | **GPIO 17** | Connects to hardware TX2 |
| **LED 1 (Red
