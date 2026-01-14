---
layout: post
title: "Building a Smart Temperature Monitor with ESP32"
date: 2026-01-12
category: IoT
description: "Learn how to create a WiFi-enabled temperature and humidity monitor using an ESP32 microcontroller and DHT22 sensor."
---

The ESP32 is an incredibly versatile microcontroller with built-in WiFi and Bluetooth. Today, we'll build a temperature and humidity monitor that sends data to your Home Assistant instance.

## Hardware Required

- ESP32 development board
- DHT22 temperature/humidity sensor
- 10kΩ resistor
- Breadboard and jumper wires
- Micro USB cable

## Circuit Diagram

Connect the components as follows:

- DHT22 VCC → ESP32 3.3V
- DHT22 GND → ESP32 GND
- DHT22 DATA → ESP32 GPIO4 (with 10kΩ pull-up resistor to 3.3V)

## Arduino Code

First, install the required libraries in Arduino IDE:
- DHT sensor library by Adafruit
- ESPAsyncWebServer

Here's the complete code:

```cpp
#include <WiFi.h>
#include <ESPAsyncWebServer.h>
#include <DHT.h>

// WiFi credentials
const char* ssid = "YOUR_WIFI_SSID";
const char* password = "YOUR_WIFI_PASSWORD";

// DHT22 configuration
#define DHTPIN 4
#define DHTTYPE DHT22
DHT dht(DHTPIN, DHTTYPE);

// Web server
AsyncWebServer server(80);

// Variables to store sensor data
float temperature = 0;
float humidity = 0;
unsigned long lastUpdate = 0;

void setup() {
  Serial.begin(115200);
  
  // Initialize DHT sensor
  dht.begin();
  
  // Connect to WiFi
  WiFi.begin(ssid, password);
  Serial.print("Connecting to WiFi");
  
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  
  Serial.println("");
  Serial.println("WiFi connected");
  Serial.print("IP address: ");
  Serial.println(WiFi.localIP());
  
  // Setup web server routes
  setupServer();
  
  // Start server
  server.begin();
  Serial.println("HTTP server started");
}

void loop() {
  unsigned long currentMillis = millis();
  
  // Update sensor readings every 2 seconds
  if (currentMillis - lastUpdate >= 2000) {
    lastUpdate = currentMillis;
    readSensors();
  }
}

void readSensors() {
  float newTemp = dht.readTemperature();
  float newHumidity = dht.readHumidity();
  
  if (!isnan(newTemp) && !isnan(newHumidity)) {
    temperature = newTemp;
    humidity = newHumidity;
    
    Serial.printf("Temperature: %.1f°C, Humidity: %.1f%%\n", 
                  temperature, humidity);
  } else {
    Serial.println("Failed to read from DHT sensor!");
  }
}

void setupServer() {
  // Root endpoint - HTML dashboard
  server.on("/", HTTP_GET, [](AsyncWebServerRequest *request){
    String html = buildDashboard();
    request->send(200, "text/html", html);
  });
  
  // JSON endpoint for Home Assistant
  server.on("/api/sensors", HTTP_GET, [](AsyncWebServerRequest *request){
    String json = buildJSON();
    request->send(200, "application/json", json);
  });
  
  // Individual sensor endpoints
  server.on("/api/temperature", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send(200, "text/plain", String(temperature));
  });
  
  server.on("/api/humidity", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send(200, "text/plain", String(humidity));
  });
}

String buildJSON() {
  String json = "{";
  json += "\"temperature\":" + String(temperature) + ",";
  json += "\"humidity\":" + String(humidity) + ",";
  json += "\"unit_temperature\":\"celsius\",";
  json += "\"unit_humidity\":\"percent\"";
  json += "}";
  return json;
}

String buildDashboard() {
  String html = "<!DOCTYPE html><html><head>";
  html += "<meta charset='UTF-8'>";
  html += "<meta name='viewport' content='width=device-width, initial-scale=1.0'>";
  html += "<title>ESP32 Climate Monitor</title>";
  html += "<style>";
  html += "body{font-family:monospace;background:#0a0e14;color:#d9dfe8;padding:20px;}";
  html += ".container{max-width:600px;margin:0 auto;}";
  html += "h1{color:#00e676;border-bottom:2px solid #00e676;padding-bottom:10px;}";
  html += ".sensor{background:#131920;border:1px solid #2d3640;padding:20px;margin:20px 0;}";
  html += ".value{font-size:48px;color:#00e676;font-weight:bold;}";
  html += ".label{color:#828a96;text-transform:uppercase;font-size:14px;letter-spacing:2px;}";
  html += ".timestamp{color:#828a96;font-size:12px;margin-top:20px;}";
  html += "</style>";
  html += "<script>";
  html += "setInterval(function(){";
  html += "fetch('/api/sensors').then(r=>r.json()).then(d=>{";
  html += "document.getElementById('temp').textContent=d.temperature.toFixed(1);";
  html += "document.getElementById('hum').textContent=d.humidity.toFixed(1);";
  html += "document.getElementById('time').textContent=new Date().toLocaleString();";
  html += "});";
  html += "},2000);";
  html += "</script>";
  html += "</head><body>";
  html += "<div class='container'>";
  html += "<h1>ESP32 Climate Monitor</h1>";
  html += "<div class='sensor'>";
  html += "<div class='label'>Temperature</div>";
  html += "<div class='value'><span id='temp'>" + String(temperature) + "</span>°C</div>";
  html += "</div>";
  html += "<div class='sensor'>";
  html += "<div class='label'>Humidity</div>";
  html += "<div class='value'><span id='hum'>" + String(humidity) + "</span>%</div>";
  html += "</div>";
  html += "<div class='timestamp'>Last update: <span id='time'></span></div>";
  html += "</div>";
  html += "</body></html>";
  return html;
}
```

## Home Assistant Integration

Add this to your Home Assistant `configuration.yaml`:

```yaml
sensor:
  - platform: rest
    name: ESP32 Temperature
    resource: http://ESP32_IP_ADDRESS/api/temperature
    unit_of_measurement: "°C"
    scan_interval: 30
    
  - platform: rest
    name: ESP32 Humidity
    resource: http://ESP32_IP_ADDRESS/api/humidity
    unit_of_measurement: "%"
    scan_interval: 30
```

Replace `ESP32_IP_ADDRESS` with the IP address shown in your serial monitor.

## Power Optimization

For battery-powered deployments, use deep sleep:

```cpp
void enterDeepSleep() {
  Serial.println("Entering deep sleep for 5 minutes...");
  esp_sleep_enable_timer_wakeup(5 * 60 * 1000000); // 5 minutes in microseconds
  esp_deep_sleep_start();
}
```

## Troubleshooting

**WiFi won't connect:**
- Double-check your SSID and password
- Ensure you're using 2.4GHz WiFi (ESP32 doesn't support 5GHz)
- Try moving closer to your router

**Sensor readings are NaN:**
- Verify wiring connections
- Check that you have the pull-up resistor
- Try a different GPIO pin

**Web page won't load:**
- Verify the ESP32 IP address
- Check that both devices are on the same network
- Look for firewall issues

## Next Steps

Enhance this project by:
- Adding more sensors (pressure, light, CO2)
- Implementing MQTT for better Home Assistant integration
- Creating custom Lovelace cards
- Adding battery monitoring for portable installations

The complete code is available on my [GitHub repository](https://github.com/yourusername/esp32-climate-monitor).
