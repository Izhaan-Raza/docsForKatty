

# NodeMCU Starter Documentation

Welcome to the **NodeMCU Starter Guide**! This guide provides step-by-step instructions to help you get started with the NodeMCU, a powerful development board based on the ESP8266/ESP32 Wi-Fi SoC.

---

## 📋 **Table of Contents**

1. [Introduction](#introduction)  
2. [Features](#features)  
3. [Required Components](#required-components)  
4. [Setup Instructions](#setup-instructions)  
   - [Install Arduino IDE](#install-arduino-ide)  
   - [Configure NodeMCU in Arduino IDE](#configure-nodemcu-in-arduino-ide)  
   - [Connecting to NodeMCU](#connecting-to-nodemcu)  
5. [Flashing Your First Program](#flashing-your-first-program)  
6. [Connecting to Wi-Fi](#connecting-to-wi-fi)  
7. [Controlling via Web Interface](#controlling-via-web-interface)  
8. [Troubleshooting](#troubleshooting)  
9. [Resources and References](#resources-and-references)  

---

## 🌟 **Introduction**

NodeMCU is an open-source IoT platform that includes firmware and a hardware design for Wi-Fi-enabled microcontrollers. It is ideal for prototyping and developing IoT projects.

### Why NodeMCU?  
- Affordable and easy to use.  
- Built-in Wi-Fi for IoT applications.  
- Compatible with multiple programming environments.  

---

## ✨ **Features**

- **Processor:** ESP8266 or ESP32.  
- **Wi-Fi:** 802.11 b/g/n.  
- **Programming Languages:** Lua, Arduino C++, MicroPython.  
- **GPIO Pins:** 11 for ESP8266 and more for ESP32.  
- **Flash Memory:** 4 MB (default).  

---

## 🔧 **Required Components**

1. **NodeMCU (ESP8266/ESP32) board**.  
2. **Micro USB cable** (data-supported, not just charging).  
3. **Computer with Arduino IDE installed**.  
4. **Optional:** Sensors or actuators for your project.  

---

## 🛠️ **Setup Instructions**

### 1. Install Arduino IDE  
1. Download and install the [Arduino IDE](https://www.arduino.cc/en/software).  
2. Install necessary USB drivers (CH340 or CP2102, depending on your NodeMCU).  

### 2. Configure NodeMCU in Arduino IDE  
1. Open the Arduino IDE and go to **File > Preferences**.  
2. Add the following URL to the "Additional Board Manager URLs":  
   - For ESP8266: `http://arduino.esp8266.com/stable/package_esp8266com_index.json`  
   - For ESP32: `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`  
3. Go to **Tools > Board > Boards Manager**, search for "ESP8266" or "ESP32," and install the corresponding package.  

### 3. Connecting to NodeMCU  
1. Connect the NodeMCU to your computer using the USB cable.  
2. Select the appropriate board and port:  
   - Go to **Tools > Board** and choose **NodeMCU 1.0 (ESP-12E Module)** for ESP8266 or the appropriate ESP32 board.  
   - Go to **Tools > Port** and select the correct COM port.  

---

## 🚀 **Flashing Your First Program**

1. Open the Arduino IDE and write your first program. Example: Blink an LED.

```cpp
void setup() {
  pinMode(LED_BUILTIN, OUTPUT); // Initialize onboard LED pin
}

void loop() {
  digitalWrite(LED_BUILTIN, HIGH); // Turn the LED on
  delay(1000);                     // Wait for a second
  digitalWrite(LED_BUILTIN, LOW);  // Turn the LED off
  delay(1000);                     // Wait for a second
}
```

2. Click **Upload** (right arrow button).  
3. Wait for the "Done Uploading" message. The onboard LED should start blinking!

---

## 📡 **Connecting to Wi-Fi**

Use the following code to connect NodeMCU to a Wi-Fi network:

```cpp
#include <ESP8266WiFi.h> // Use <WiFi.h> for ESP32

const char* ssid = "YourWiFiSSID";       // Replace with your Wi-Fi SSID
const char* password = "YourWiFiPass";   // Replace with your Wi-Fi Password

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);

  Serial.print("Connecting to Wi-Fi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
  }
  Serial.println("\nConnected to Wi-Fi");
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());
}

void loop() {
  // Your main code here
}
```

---

## 🌐 **Controlling via Web Interface**

This example creates a simple web server to control an onboard LED:

```cpp
#include <ESP8266WiFi.h>  // Use <WiFi.h> for ESP32
#include <ESP8266WebServer.h>  // Use <WebServer.h> for ESP32

const char* ssid = "YourWiFiSSID";     
const char* password = "YourWiFiPass";

ESP8266WebServer server(80);  // Initialize web server on port 80
const int ledPin = LED_BUILTIN;

void handleRoot() {
  server.send(200, "text/html", "<h1>Welcome to NodeMCU</h1><a href='/on'>Turn LED ON</a><br><a href='/off'>Turn LED OFF</a>");
}

void handleOn() {
  digitalWrite(ledPin, LOW);  // Turn LED on (active LOW)
  server.send(200, "text/html", "<h1>LED is ON</h1><a href='/'>Go Back</a>");
}

void handleOff() {
  digitalWrite(ledPin, HIGH);  // Turn LED off (active LOW)
  server.send(200, "text/html", "<h1>LED is OFF</h1><a href='/'>Go Back</a>");
}

void setup() {
  Serial.begin(115200);
  pinMode(ledPin, OUTPUT);
  digitalWrite(ledPin, HIGH);  // Default LED off

  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
  }
  Serial.println("\nConnected to Wi-Fi");
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());

  server.on("/", handleRoot);
  server.on("/on", handleOn);
  server.on("/off", handleOff);
  server.begin();
  Serial.println("HTTP server started");
}

void loop() {
  server.handleClient();
}
```

### How to Use:
1. Upload the code to the NodeMCU.
2. Open the Serial Monitor to get the IP address.
3. Enter the IP address in your web browser.
4. Use the links to turn the LED ON or OFF.

---

## 🛠️ **Troubleshooting**

- **NodeMCU not detected:**  
  Install USB drivers (CH340 or CP2102).  
- **Wi-Fi connection issues:**  
  Double-check the SSID and password, and ensure your router is running on 2.4 GHz (not 5 GHz).  

---

## 📚 **Resources and References**

- [NodeMCU Documentation](https://nodemcu.readthedocs.io/)  
- [ESP8266 Community Forum](https://www.esp8266.com/)  
- [ESP32 Official Documentation](https://docs.espressif.com/)  
- [Arduino IDE Official Site](https://www.arduino.cc/en/software)  

---

Happy IoT building!
