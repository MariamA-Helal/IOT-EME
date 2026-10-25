# 🚀 Industrial Vibration Monitoring & TinyML Edge Node (EME Project)

![ESP32](https://img.shields.io/badge/Microcontroller-ESP32-E7352C?style=for-the-badge&logo=espressif&logoColor=white)
![FreeRTOS](https://img.shields.io/badge/RTOS-FreeRTOS-226699?style=for-the-badge&logo=tux&logoColor=white)
![TinyML](https://img.shields.io/badge/AI-Edge_Impulse-FF6F00?style=for-the-badge&logo=edgeimpulse&logoColor=white)
![MQTT](https://img.shields.io/badge/Protocol-MQTT-430098?style=for-the-badge&logo=mqtt&logoColor=white)
![PlatformIO](https://img.shields.io/badge/IDE-PlatformIO-000000?style=for-the-badge&logo=platformio&logoColor=white)

An advanced **Industrial IoT (IIoT) Edge Node** built with the **ESP32**, running a multi-tasking **FreeRTOS** architecture. The system rapidly samples analog vibration data (simulated via a potentiometer), computes RMS amplitude, classifies machine states using statistical thresholds or an **Edge Impulse TinyML model**, manages Wi-Fi dynamically via a captive portal, and streams live telemetry to a custom **HTML/Web Dashboard** via **Secure MQTT (TLS)**.

Developed as part of the EME training program, showcasing real-time embedded systems, machine learning on the edge, and secure industrial protocols.

---

## 🧠 System Architecture & FreeRTOS Multi-Tasking

To handle high-frequency sampling, local display updates, ML classification, and network streaming simultaneously without blocking, the firmware implements an **Option A 4-Task Structure** using **FreeRTOS**:

1. **Rapid Sampling Task (High Priority):** Continuously collects analog samples at 100Hz from the potentiometer (simulating machine vibration).
2. **Processing & RMS Computation Task:** Computes RMS amplitude and evaluates machine health state (**NORMAL / ELEVATED / FAULT**) using statistical thresholds or the embedded TinyML model.
3. **Actuation & Local Feedback Task:** Updates the SSD1306 OLED display with live status and confidence percentages, while triggering the passive buzzer and external LED alarms instantly upon detecting a `FAULT`.
4. **Communication & MQTT Task (Low Priority):** Manages Wi-Fi connection states, handles the **WiFi Manager** captive portal fallback, and publishes secure payloads via **TLS MQTT** to the dashboard.

---

## 🌟 Key Features

- **High-Frequency Analog Sampling:** Rapidly samples vibration signals at 100Hz using ESP32 ADC.
- **Edge Machine Learning (TinyML):** Integrated Edge Impulse classification model for anomaly detection directly on the microcontroller.
- **Dynamic Wi-Fi Provisioning:** Embedded Captive Portal (**WiFi Manager**) allowing seamless SSID configuration without hardcoding credentials.
- **Secure Industrial Telemetry:** Publishes classification results, confidence scores, and raw RMS values securely via MQTT (TLS).
- **Custom HTML Dashboard:** A lightweight web interface to monitor live machine vibration states and telemetry data in real-time.
- **Local Alerts & UI:** Visual and audible feedback using an SSD1306 OLED display, external LED, and passive buzzer.

---

## 🛠️ Hardware Requirements & Pinout Connections

### Component List
* 1 × **ESP32 DevKit** (Main Controller)
* 1 × **Potentiometer** (Simulates vibration amplitude)
* 1 × **SSD1306 OLED Display** (128x64, I2C)
* 1 × **External LED** (Status / Fault indicator)
* 1 × **Passive Buzzer** (Audible alarm)
* Breadboard, Jumper wires, and USB cable

### Pinout & Connections Table
| Component | Pin on Component | ESP32 Pin | Description |
| :--- | :--- | :--- | :--- |
| **Potentiometer** | SIG (Output) | `GPIO 34` (ADC1_CH6) | Analog vibration simulation input |
| **Potentiometer** | VCC / GND | `3V3` / `GND` | Power supply |
| **OLED Display** | SDA | `GPIO 21` | I2C Data line |
| **OLED Display** | SCL | `GPIO 22` | I2C Clock line |
| **External LED** | Positive (+) | `GPIO 25` (or configured pin) | Fault warning indicator |
| **Passive Buzzer** | Positive (+) | `GPIO 25` | Audible fault alarm |

---

## 📁 Project Directory Structure

```text
IOT-EME/
├── include/                 # 📂 Header files (.h) for configurations and drivers
├── src/                     # 🚀 Main source files (.cpp / .c)
│   ├── main.cpp             # FreeRTOS tasks setup & initialization
│   ├── wifi_manager.cpp     # Captive portal & Wi-Fi provisioning logic
│   ├── ml_model.cpp         # Edge Impulse TinyML integration
│   └── sensor_sampling.cpp  # 100Hz sampling & RMS calculation
├── dashboard/               # 🌐 HTML / Web Dashboard files for live monitoring
├── platformio.ini           # ⚙️ PlatformIO project configuration & build flags
└── README.md                # 📖 Project documentation
```

