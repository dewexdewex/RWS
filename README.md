# RWS
Rear wheel steer trailer project

Code for two ESP32 Feather boards. One designated as Master, the other as Slave.

The Master board is attached to the bicycle. It takes input from switches for On/Off, Reset and a Hall Effect sensor. The Master has output LEDs to indicate state and, when in operation, outputs a floating point number which is sent wirelessly as input to the Slave board using ESPNOW.

The Slave board takes input from On/Off, Reset buttons and a float number from the Master board via P2P wireless connection using ESPNOW protocol. It outputs a control voltage to a mechanical servo, which changes the steering angle of a free wheel on the rear trailer unit to work in the opposite sense to the bicycle steering.

# Board MAC addresses

with:

#include <WiFi.h>

void setup() {
  Serial.begin(115200);
  WiFi.mode(WIFI_MODE_APSTA);
  delay(1000);
  Serial.print("MAC address: "); Serial.println(WiFi.macAddress());
}

void loop() {}

Master (M) (ESP32 FEATHER) MAC Address: 40:91:51:1F:25:48

Slave (S) (ESP32 FEATHER) MAC Address: 8C:4B:14:09:AE:CC >>>>> This Board is DEAD

Older board (A) (ESP32 FEATHER) with headers MAC Address: 30:AE:A4:F2:C1:8C

Older board (B) (ESP32 FM DEVKIT) with headers MAC Address: 30:AE:A4:D3:2D:34 >>>>> This Board is DEAD

N1: (ESP32 Dev Module) with headers MAC Address: 08:D1:F9:9A:77:18 This board needs the upload speed to be low.
N2: (ESP32 Dev Module) with headers MAC Address: 08:D1:F9:D9:4A:D4 This board needs the upload speed to be low.

I used ChatGPT to generate some code examples. The public URL for the chat is: https://chatgpt.com/share/eae55703-89e8-4bb3-85e4-c148a3af827c

# ESP32Servo and ESP Board Manager compatibility issues

I spent all day getting a combination of Board managers to give me a Sketch which includes the ESP32Servo library to compile:

These were: 
ArduinoESP32 Boards v 2.0.13
esp32 v2.0.14

# ESP Board failures

I am destrying ESP boards by connecting them to the servo and the 7.4v battery. I believe I need some form of isolation using a decoupling capacitor to prevent this from happening further.

