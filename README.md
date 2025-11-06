# Aladin
![Aladin](https://media.discordapp.net/attachments/662375257591513088/1436133799879774259/imagem_2025-11-06_202136005-removebg-preview.png?ex=690e7f53&is=690d2dd3&hm=d73dd82f23c506954707610a3d3c220d6c6cff4e363ab179a7005aa09bac49ba&=&format=webp&quality=lossless)   
This project implements a smart home automation system using an **ESP32**, a **relay module**, and **Firebase Realtime Database**.
It provides a secure web interface hosted directly on the ESP32, allowing users to control a lamp through **voice commands** (processed by Google's Gemini AI) or manual interaction.

All lamp state changes are logged to Firebase, including timestamps, energy consumption, and estimated cost, calculated in real time based on the lamp’s power rating.

---

## Overview

The ESP32 serves as both the web server and IoT controller.
Users can access a local HTTPS page to issue commands, switch between manual and automatic operation modes, and monitor the system’s behavior.
In automatic mode, the lamp toggles periodically, and all data is recorded to Firebase for later analysis.

---

## Features

* Voice-controlled lamp switching via **Gemini AI**
* Local HTTPS web interface hosted on the ESP32
* **Manual** and **Automatic** operation modes
* Real-time logging to **Firebase Realtime Database**
* Automatic time synchronization using **NTP**
* Calculation of **energy consumption (kWh)** and **cost**

---

## Hardware Requirements

* ESP32 development board
* Single-channel 5V relay module
* 5V lamp or LED load
* USB cable and power supply
* Wi-Fi network access

---

## Software Requirements

* **Arduino IDE** (latest version)
* **ESP32 board package** (Espressif Systems)
* Libraries:

  * `Firebase ESP32 Client` by Mobizt
  * `NTPClient` by Fabrice Weinberg
  * `WiFi.h` (included with ESP32 package)

---

## Firebase Setup

1. Go to [Firebase Console](https://console.firebase.google.com/).
2. Create a new project and enable **Realtime Database**.
3. Set the database to test mode (for local testing).
4. Copy your **Database URL** (format: `https://your-project-id.firebaseio.com/`).
5. In **Project Settings → Service Accounts**, generate a new **API key**.
6. Replace the placeholders in the code:

   ```cpp
   #define API_KEY "your_firebase_api_key"
   #define DATABASE_URL "https://your-project-id.firebaseio.com/"
   ```

---

## Gemini AI Setup

1. Create an API key at [Google AI Studio](https://aistudio.google.com/app/apikey).
2. Replace it in the HTML section of the code:

   ```javascript
   const GEMINI_API_KEY = "your_gemini_api_key";
   ```

---

## How It Works

1. The ESP32 connects to Wi-Fi and starts an HTTPS server.
2. The NTP client synchronizes the time automatically.
3. The user accesses the ESP32’s IP address in a browser (`https://<device_ip>`).
4. Voice commands such as “Turn on the light” or “Turn off the light” are interpreted by Gemini AI.
5. The ESP32 activates or deactivates the relay accordingly.
6. Every change is logged to Firebase with:

   * Lamp state (`ON` / `OFF`)
   * Timestamp
   * Energy consumption (kWh)
   * Estimated cost (in local currency)

---

## Data Example in Firebase

```json
{
  "lamp": {
    "atual": "LIGADA",
    "historico": {
      "-OyzX12abc": {
        "estado": "DESLIGADA",
        "timestamp": "14:32:10",
        "tempo_h": 0.0042,
        "energia_kwh": 0.00003,
        "custo_reais": 0.00002
      }
    }
  }
}
```

---

## Automatic Mode

When automatic mode is enabled, the ESP32 toggles the lamp every 15 seconds and logs each change to Firebase.
This mode can be used for testing, simulation, or energy measurement experiments.

---

## Notes

* The HTTPS server requires valid **certificate** and **private key** to operate.
* All communication with Firebase uses secure HTTPS.
* The project was tested on ESP32 DevKit v1 with Arduino IDE 2.3.2.

---

## License

This project is distributed under the MIT License.
You are free to use, modify, and distribute it for educational or research purposes.
