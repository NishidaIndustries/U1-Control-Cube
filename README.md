# U1-Control-Cube

The **U1-Control-Cube** is a compact, wireless desktop gadget designed to let you monitor and control **five separate Snapmaker U1 printers** using simple physical movements. 

Instead of juggling multiple computer screens or web tabs to check on your print farm, you simply pick up the cube and rotate it on your desk.

---

## 🕹️ What It Does
The cube features **five small screens** (Top, Left, Right, Front, and Back). Each face is dedicated to tracking one specific Snapmaker U1 printer in your workshop. 

When a face is rotated upward, its screen instantly displays live information for that specific printer:
* ⏳ **Print Status:** See if the machine is printing, idle, paused, or offline.
* 📈 **Progress & Time:** Check the completion percentage and how much printing time is left.
* 🧵 **Filament Remaining:** View exactly how many grams of filament are left on the spool so you can prevent unexpected runouts.
* 🌡️ **Temperature:** Check real-time heat readouts for the active nozzle.
* 💵 **Live Print Cost:** Automatically calculates how much money the active print has cost you so far based on filament usage and local electricity rates.

---

## ⚙️ How It Works
The magic of the cube relies on a few simple, smart components tucked inside its compact frame:

1. **5 Small Screens:** Five independent, sharp 0.96-inch OLED display modules built directly into the faces of the cube structure.
2. **The Brain (ESP32-S3):** A tiny microchip that connects directly to your studio's Wi-Fi network. It constantly talks to your printers in the background to grab live updates.
3. **The Motion Sensor (MPU6050):** An internal gravity sensor (accelerometer) that detects which way the cube is tilted. When you turn the cube, the sensor figures out which face is pointing up.
4. **Smart Power Management:** To save battery life, the cube only fully activates the screen that is currently facing up. The other four screens stay asleep until you rotate them into view!

---

## 💾 Core Firmware Code

*Code files are empty for initial repo setup. Implementation versions will be pushed directly to this block in future milestones.*

```text
// [WIP] Firmware Source Code Structure to be Added.
// Core logic will manage background Wi-Fi telemetry streams from Moonraker APIs,
// handle MPU6050 orientation filtering, and update the 5 independent small displays.
```

---

## 🛠️ License
This project is open-source under the MIT License framework.
