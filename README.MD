# ESP8266 IoT Device - Firebase Data Sender

This project demonstrates how to use an **ESP8266** to simulate sensor data (temperature and humidity), generate a timestamp in **GMT+7**, and send the data to a **Firebase Realtime Database** using **HTTPS PUT requests**. It also uses **WiFiManager** to allow WiFi configuration if the ESP cannot connect automatically.

---

## Features

- Connects to WiFi automatically or uses **WiFiManager** if connection fails.
- Generates **random temperature and humidity** values (simulating a sensor).
- Updates **timestamp in ISO 8601 format** (GMT+7).
- Sends device data to **Firebase Realtime Database**.
- Uses **HTTPS** for secure communication.
- Customizable **device ID** to differentiate multiple devices.

---

## Libraries Required

Install the following libraries in the Arduino IDE:

1. **ArduinoJson** by Benoit Blanchon, version 7.4.2  
   [https://arduinojson.org/](https://arduinojson.org/)

2. **Firebase ESP8266 Client** by Mobizt, version 4.4.17  
   [https://github.com/mobizt/Firebase-ESP8266](https://github.com/mobizt/Firebase-ESP8266)

3. **WiFiManager** by tzapu  version 2.0.17
   [https://github.com/tzapu/WiFiManager](https://github.com/tzapu/WiFiManager)

---

## Hardware Required

- **ESP8266** (e.g., NodeMCU, Wemos D1 Mini)
- USB cable for programming
- Optional: DHT22 or any other sensor if you want to replace simulated data

> In this project, the sensor is simulated with random values.

---

## Setup

1. Clone or copy the Arduino sketch into the Arduino IDE.
2. Install the required libraries listed above.
3. Update the **WiFi credentials** in the sketch:

```cpp
#define WIFI_SSID "your_wifi_ssid"
#define WIFI_PASSWORD "your_wifi_password"

## UpdateFirebaseUR :
String FIREBASE_HOST = "https://your-project-id.firebaseio.com";

## Update device ID if desired:
DeviceData dataSensor = 
{
  "esp-dht22-04",  // device_id
  "",               // timestamp (auto-generated)
  0.0,              // temperature
  0.0               // humidity
};

## Upload the code to your ESP8266 board.
How it Works

The ESP attempts to connect to the WiFi network using the saved credentials.

If the connection fails, WiFiManager starts an access point with the device name (e.g., esp-dht22-04) so you can select your WiFi network.

Once connected, the ESP:

Generates a random temperature (20–30°C) and humidity (40–60%).

Generates a timestamp in ISO 8601 format with GMT+7 offset.

Sends the data as a JSON object to Firebase via HTTPS PUT.

The process repeats every 10 seconds.

##JSON Format Sent to Firebase
{
  "device_id": "esp-dht22-04",
  "timestamp": "2025-09-05T21:00:00+07:00",
  "temperature": 27.5,
  "humidity": 50.2
}
## Notes

Make sure your Firebase database has the correct rules to allow PUT requests.
Example:

{
  "rules": {
    ".read": true,
    ".write": true
  }
}
If using an actual DHT sensor, you can replace the random data generation with dht.readTemperature() and dht.readHumidity().

The ESP uses BearSSL::WiFiClientSecure with .setInsecure() to bypass certificate verification for HTTPS.

