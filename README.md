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

N1: (ESP32 Dev Module) with headers MAC Address: 08:D1:F9:9A:77:18 This board needs the upload speed to be low. Connecting with driver /dev/cu.wchusbserial56BA0198571 Serial Port (USB)


N2: (ESP32 Dev Module) with headers MAC Address: 08:D1:F9:D9:4A:D4 This board needs the upload speed to be low.

I used ChatGPT to generate some code examples. The public URL for the chat is: https://chatgpt.com/share/eae55703-89e8-4bb3-85e4-c148a3af827c

# ESP32Servo and ESP Board Manager compatibility issues

I spent all day getting a combination of Board managers to give me a Sketch which includes the ESP32Servo library to compile:

These were: 

ArduinoESP32 Boards v 2.0.13

esp32 v2.0.14

On the Catapult Macbook, I had compilation issues with the latest version of the ESP32Servo lirary latest version, so I installed v1.1.0 and this version works with the versions of ArduinoESP32 and esp32 quoted above.

# ESP Board failures

I am destrying ESP boards by connecting them to the servo and the 7.4v battery. I believe I need some form of isolation using a decoupling capacitor to prevent this from happening further.

# New cheap board issues

Every new upload requires the upload speed to be dropped to the lowest possible, or upload will break on these new cheapo boards.

I had issues connecting with the Silicon Labs USB drivers on the Catapult Macbook, so installed the CH34x drivers and these worked.

# Requirements for ChatGPT
Output here: https://chatgpt.com/share/11db7cfa-bb93-4bfa-aa79-9afaa62ad0d6

The primary objective is to build a rear wheel steered trailer system for a bicycle. The rear wheel steering is done remotely and wirelessly using ESP32 boards in a master slave configuration with ESPNOW. The system automatically measures steering angle from the bicycle and sends this to the servo on the rear wheel steer unit to alter its steering angle to make negotiation of corners more smoothe. 

# Hardware:
ESP Master board;

ESP Slave board;

Hall Effect Sensor for measurement of bicycle steering angle;

Hall Effect Sensor for measurement of trailer wheel steering angle;

Servo for controlling the angular position of the automatically steered trailer wheel;

Smartphone for displaying data served from a web server running on the ESP master board.

# ESP Master board operation:

Has a momentary switch connected to a GPIO pin to invoke the Reset Wizard sequence.

Periodically automatically receives input of a linear analogue voltage from the Hall Effect Sensor for measurement of bicycle steering angle;

Sends output of the linear analogue voltage from the Hall Effect Sensor for measurement of bicycle steering angle, using ESPNOW, to the ESP Slave board;

Receives output of the linear analogue voltage from the Hall Effect Sensor for measurement of trailer wheel steering angle, using ESPNOW, from the ESP Slave board;
Uses the difference bewteen the two steering positions in a PID type closed loop control system to help correct errors between the required and actual values of the input bicycle steering angleand the output trailer wheel steering angle;

Is configured as an accesspoint and serves one of two web pages for display on a nearby smart phone connected to the access point. 

The main web page, called the Running View, displays: The on/off/reset status of the system; The charge level of the LiPo batteries on the ESP Master and the ESP Slave board; Numerical values shown as bars, which represent the bicycle steering angle and the trailer wheel steering angle, so that they are adjacent for comparison; Sliders with a range of values for parameters of the PID type closed loop control system; A button to send any changed values for parameters of the PID type closed loop control systemback to the ESP Master board; a button labeled Reset, which causes the second web page, the Reset Wizard, to be displayed. The web page should produ ce an adible alert if either of the LiPo batteries falls below 80% charge.

The second web page, the Reset Wizard, displays the instructional steps for the system reset sequence as a multistep wizard. It is invoked when the user either presses the momentary switch connected to the ESP Master board, or taps a button on the web pages. The steps are: "Checking batteries"; "Set steering of bicycle to central position and click here when this is complete". 

The reset Wizard is also used as the Startup Wizard, when the system is fully powered up.

# ESP Slave board operation:

Receives as input, output of the linear analogue voltage from the Hall Effect Sensor, from the ESP Master board, which represents the bicycle steering angle, and converts this angle value to a PWM signal, suitable for the steering Servo, which then causes the Servo to change the steering angle position of the rear steering wheel to be the opposite match to the current bicycle steering angle position.

Receives input from a second Hall Effect sensor as a linear analogue voltage, which represents the current steering angle of the steered trailer wheel. Simultaneously sends this as a value of steering angle to compare with the current bicycle steering angle, for error correction.

Sends battery charge status back to the ESP Master board periodically and automatically for display on the Running view on the smartphone system web page.




