# U1-Control-Cube

An open-source, wireless desktop hardware command node designed for multi-machine setups running Klipper/Moonraker firmware—specifically optimized for managing up to five **Snapmaker U1** toolchanger units simultaneously. 

By utilizing an **ESP32-S3** microcontroller paired with a 6-axis **MPU6050** inertial measurement unit (IMU), the cube dynamically monitors its physical orientation. Rotating a specific face upward seamlessly wakes up its corresponding independent SPI OLED display screen, pulls real-time JSON telemetric payloads directly over the local network via asynchronous Web requests, and exposes low-latency remote execution parameters (Pause, Stop, Preheat).

---

## 📦 Key Technical Repository Architecture

```text
├── firmware/
│   ├── U1_ControlCube.ino      # Multi-display SPI driver & accelerometer orientation engine
│   └── config.h                # Network credentials, local IP tables, and cost variables
├── hardware/
│   ├── casing_frame.stl        # 3D-printable minimal internal matrix mount framework
│   └── gold_accent_trim.stl    # Outer aesthetic structural alignment guards
├── assets/
│   └── makitlab_logo.png       # Official project deployment branding asset
└── README.md                   # System configuration and structural wire mapping
```

---

## ⚡ Hardware Layout & Interfacing Map

The system relies on six independent 0.96-inch or 1.3-inch SPI OLED screens sharing a uniform high-speed hardware Data/Clock bus to minimize core signal routing traces. Machine state updates are mapped dynamically based on which independent Chip-Select (`CS`) line is toggled by the spatial filter logic.

### 🔌 Microcontroller Pin Allocation Configuration

| Component Module | Signal Protocol | ESP32-S3 Pin Link | Pin Operating Logic |
| :--- | :--- | :--- | :--- |
| **Shared Displays** | `SPI MOSI` | **GPIO 11** | Master Out Slave In Shared Data |
| **Shared Displays** | `SPI SCK` | **GPIO 12** | Synchronous Clock Timing Bus |
| **Shared Displays** | `SPI D/C` | **GPIO 13** | Common Data / Command Selection |
| **Face 1 (Node A)** | `SPI CS 1` | **GPIO 1** | Node A Display Chip Enable |
| **Face 2 (Node B)** | `SPI CS 2` | **GPIO 2** | Node B Display Chip Enable |
| **Face 3 (Node C)** | `SPI CS 3` | **GPIO 3** | Node C Display Chip Enable |
| **Face 4 (Node D)** | `SPI CS 4` | **GPIO 4** | Node D Display Chip Enable |
| **Face 5 (Node E)** | `SPI CS 5` | **GPIO 5** | Node E Display Chip Enable |
| **Face 6 (Dock Station)**| `SPI CS 6` | **GPIO 6** | Overview Status Monitor Enable |
| **MPU6050 Accelerometer**| `I2C SDA` | **GPIO 8** | Gravity Vector Data Stream |
| **MPU6050 Accelerometer**| `I2C SCL` | **GPIO 9** | Inertial Clock Serial Timing |

---

## 🎨 Spatial Interaction Matrix Mapping

```text
               +-------------------+

               |    FACE 5 UP      |
               |   [ U1 NODE E ]   |
               +-------------------+
                        │  Flipped Forward
                        ▼
+-------------------+---------+-------------------+---------+

|    FACE 4 UP      |         |    FACE 3 UP      |         |
|   [ U1 NODE D ]   |         |   [ U1 NODE C ]   |         |
+-------------------+         +-------------------+         |

|    FACE 2 UP      |         |    FACE 1 UP      |         |
|   [ U1 NODE B ]   |         |   [ U1 NODE A ]   |         |
+-------------------+---------+-------------------+---------+
                        ▲
                        │  Flipped Backward
               +-------------------+

               |    FACE 6 UP      |
               |  [ MASTER DOCK ]  |
               +-------------------+
```

* **Faces 1 to 5 (Printer Focus Nodes):** Displays real-time operational layers, active structural nozzle/bed thermal profiles, total filament metrics processed, and accurate real-time currency overhead tracking.
* **Face 6 (Chameleon Base Hub):** Placed down on the magnetic charging dock to immediately yield a comprehensive macro-level farm overview, aggregating online states and total active print queues.

---

## 🔧 Installation & Dependencies

To compile and flash the firmware located inside the `/firmware` directory using the Arduino IDE or PlatformIO, make sure the following peripheral libraries are installed via the library manager:

1. `Adafruit_MPU6050` (Inertial sampling core)
2. `Adafruit_SSD1306` & `Adafruit_GFX` (Asynchronous hardware display drivers)
3. `ArduinoJson` (v7.x - Optimized memory pool management for incoming Moonraker JSON string arrays)

---

## 📡 Live Telemetric API Communication Hook

The internal network client constructs non-blocking HTTP streaming channels directly to each Snapmaker toolchanger's background engine by tracking standard Klipper endpoint objects:

```http
GET http://<U1_NODE_IP>/printer/objects/query?print_stats&toolhead&extruder&heater_bed
```

The underlying mathematical logic extracts total filament mass extruded in grams alongside active elapsed timeline counters to calculate live cost accumulation:

$$\text{Total Print Cost} = (\text{Grams Extruded} \times \text{Cost per Gram}) + \left(\frac{\text{Total Duration (sec)}}{3600} \times \text{Average Wattage (kW)} \times \text{Local Tariff Rate}\right)$$

---

## 🛠️ Contribution & Development License

This system configuration is fully open for community adaptations and modifications under the MIT License framework. Feel free to submit pull requests to enhance Klipper macro integrations, update display layouts, or optimize multi-threaded network handling loops. Built for makers who believe utility should match a clean desk aesthetic.
