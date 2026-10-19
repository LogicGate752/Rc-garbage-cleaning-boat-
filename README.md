# 🚢 ESP32 Wi-Fi Smart RC Garbage Skimmer Boat

An open-source, eco-friendly utility craft designed to autonomous/manually skim and collect floating surface debris and plastic waste from water bodies. Powered by an **ESP32 microcontroller**, it hosts its own standalone Wi-Fi hotspot control network, streams an interactive web interface controller, and uses **Ultrasonic Telemetry arrays** to compute storage bin levels in real-time.

---

## ✨ Features
* 📶 **Direct Wi-Fi Access Point:** Generates its own independent local control network (`RC_Trash_Boat`). No internet router required.
* 🕹️ **Zero-Install Web App Dashboard:** Zero external applications needed—drive the craft via an optimized custom browser interface.
* ♻️ **Live Debris Tracking:** Real-time distance measurement tracking scales cargo container limits directly into an active live percentage display meter.
* ⚡ **Fast Compile Memory Struct:** Leverages optimized `PROGMEM` data arrays to bypass local compilation lags completely.

---

## 🛠️ Bill of Materials (BOM)

| Component | Function | Qty |
| :--- | :--- | :--- |
| **ESP32 NodeMCU DevKit V1** | Main Microprocessor Core | 1 |
| **L298N Module** | Dual H-Bridge DC Motor Driver Module | 1 |
| **HC-SR04** | Ultrasonic Telemetry Distance Sensor | 1 |
| **Standard SG90/MG996R** | Steering Rudder Actuator Servo | 1 |
| **130/380 Toy DC Motors** | Propeller Propulsion Actuators | 2 |
| **LM2596 Buck Converter** | 5V Main Power Step-Down Regulator | 1 |
| **7.4V - 12V Li-Po / 18650 Pack** | Primary System Battery Fuel Supply | 1 |

---

## 🔌 Master Hardware Wiring Map

```text
                  +--------------------------------+

                  |      Main Battery Pack         |
                  |     (7.4V - 12V DC Source)     |
                  +---------------+----------------+
                                  |
                   +--------------+---------------+

                   |                              |
                   v                              v
        +--------------------+          +--------------------+

        |   Buck Converter   |          |    L298N Driver    |
        |   (IN+ / IN-)      |          |    (12V / GND)     |
        +---------+----------+          +---------+----------+

                  |                               |
                  v (Regulated 5V Output)         | (Common Ground Connection)
        +-----------------------------------+     |

        |  ESP32 VIN  | Sensor VCC | Servo  |<----+
        +-----------------------------------+
```

### 1. Power Traces & Common Grounding
* **Battery Pack (+)** ➔ Buck Converter **IN+** *AND* L298N **12V Screw Terminal**
* **Battery Pack (-)** ➔ Buck Converter **IN-** *AND* L298N **GND Screw Terminal**
* **Buck Converter OUT+ (Set to 5V)** ➔ ESP32 **VIN** *AND* Ultrasonic **VCC** *AND* Servo **Red Wire**
* **Buck Converter OUT-** ➔ ESP32 **GND Pin** *(Establishes mandatory shared logic common ground reference)*

### 2. Logic Control Traces
* **L298N ENA (Left Speed)** ➔ ESP32 **GPIO 25** *(Remove jumper cap, plug into inner pin)*
* **L298N IN1 / IN2** ➔ ESP32 **GPIO 26 / GPIO 27**
* **L298N IN3 / IN4** ➔ ESP32 **GPIO 32 / GPIO 33**
* **L298N ENB (Right Speed)** ➔ ESP32 **GPIO 18** *(Remove jumper cap, plug into inner pin)*
* **Steering Servo Signal (Orange/Yellow)** ➔ ESP32 **GPIO 13**
* **Ultrasonic Trig / Echo** ➔ ESP32 **GPIO 12 / GPIO 14**

### 3. Actuator Outputs
* **Left Motor** ➔ L298N **OUT1 / OUT2** Screw Ports
* **Right Motor** ➔ L298N **OUT3 / OUT4** Screw Ports

---

## 💻 Firmware Setup (Arduino IDE)

### 📚 Core Dependencies
To build the sketch locally, search for and install this single external manager package inside your IDE (**Tools ➔ Manage Libraries...**):
* `ESP32Servo` *(by Kevin Harrington)*

### ⚙️ Target Settings
1. Add the Espressif indexing framework link under **File ➔ Preferences ➔ Additional boards manager URLs**:
   `https://github.io`
2. Go to **Tools ➔ Board** and select **`DOIT ESP32 DEVKIT V1`**.
3. Set **Upload Speed** to `115200` and **Flash Frequency** to `80MHz`.

---

## 🕹️ Operations Guide
1. Turn on the physical battery power switch. The buck converter status LED will light up solid.
2. Grab your smartphone, go to **Wi-Fi Settings**, and connect to the local device network:
   * **SSID:** `RC_Trash_Boat`
   * **Password:** `password123`
3. Launch any mobile web browser application (e.g., Safari, Chrome) and type **`192.168.4.1`** into the top navigation search bar.
4. Use the arrow buttons to drive, adjust the range slider to steer the rudder, and keep an eye on the trash fill-gauge to maximize your skimmer's cleanup efficiency!

---

## 📝 License
This project is open-source and available under the **MIT License**. Feel free to fork, hack, modify, and build upon it!
