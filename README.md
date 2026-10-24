<<<<<<< HEAD
# 🌎 ESP32 Environmental Monitoring System 🚀

This project is WiFi-enabled real-time environmental monitoring system built on **ESP32 WROOM 32D** microcontroller. It captures and displays key atmospheric parameters such as **temperature**🌡️, **humidity**💧, **PM2.5 dust density**🌫️, **dust levels**💨, **air PPM levels**🏭 and **air quality index(AQI)⚠️** using various sensors. This data is displayed locally on a **128x64 OLED display** and also served through a responsive **web interface** hosted on ESP32 itself over the local network.

 
## 🚀 Features
- 📡 **WiFi Based Server**: ESP32 hosts a dynamic webpage accessible to any device on the same network acting as a webserver
- 🌡️ **Temperature monitoring**: Captures accurate ambient temperature using digital temperature sensor
- 💧 **Humidity Monitoring**: uses humidity sensor for relative humidity data
- 🌫️ **PM2.5 Air Quality Measurement**: Uses dust density sensor to estimate PM2.5 levels
- 💨 **Dust Level**: Based on PM2.5 level, it classifies the value into dust density levels
- 🏭 **Air PPM Levels**: Uses air quality/gas detector sensor to estimate PPM levels
- ⚠️ **AQI Level**: Based on PPM level, it categorizes the value into Air Quality Index
- 🕙 **Live Clock with NTP**: Fetches real time date and time using NTP(Network Time Protocol)
- 📺 **OLED Display Output**: Display the readings on a OLED screen
- 🔁 **Timed sensor Updates**: Reads and updates sensor values after every 60 seconds
- 🌐 **Responsive Web Dashboard**: Presents clean and styled HTML output of live data


## 🔧 Hardware Used
| 🧩 Component | 🔍 Description | 📦 Model/Type |
|--------------|----------------|--------------|
| 🧠 Microcontroller | Main controller of the system - with WiFi & Bluetooth | ESP32 WROOM 32D IoT development board|
| 🧠 Secondary Microcontroller | Main controller - To calibrate air quality/dust detector sensor | Arduino UNO Rev3 development board|
| 🌡️ Temperature Sensor | To measure ambient temperature| Dallas Semiconductors DS18B20 Digital Temperature Sensor (Breakout Board)|
| 💧 Humidity Sensor | To measure ambient humidity | DHT11 Sensor (Breakout Board) |
| 🌫️ Dust Sensor | To measure suspended 2.5um particulate matter in air| PM2.5 GP2Y1010AU0F Dust Smoke Particle Sensor |
| 🌫️ Air Quality/Gas Detection Sensor | To measure air quality & Parts Per Million in Air| MQ-135 Air Quality/Gas Detection Sensor (Breakout Board) |
| 📺 Display Screen | To display sensor readings locally | SSD1306 driver based 0.96" 7Pin 128x64 monochromatic(Blue) SPI/I2C OLED display (Breakout Board) |
| 150Ω resistor + 220µF Capacitor  | Resistor to limit current in IR LED, Capacitor for signal smoothening | For PM2.5 GP2Y1010AU0F Dust Smoke Particle Sensor |
| 2x 10kΩ resistor  | Resistors act as voltage divider to protect ESP32 Analog pins from voltage surge from analog pin of the sensor | For MQ-135 Air Quality/Gas Detection Sensor |
| 🔌  Electrical Components | Prototyping components | Dupont Jumper wires(M-M, M-F, F-F), Breadboard, Acrylic board|
| 🔋 Power Supply| To power the whole system | Laptop USB adapter |
| 🛠️ Multimeter | For voltage, current & continuity testing | Kaiweets HT118E 20000 count Digital Multimeter |
| 🪛 Soldering Iron | For assembling and soldering components | 25W soldering iron |


## 📐 Pin Configuration
| Module | Pin Name | ESP32 GPIO |
|--------|----------|------------|
| OLED Display (SPI) | VCC | 5V |
|  | GND | GND |
|  | MOSI | GPIO 23 |
|  | CLK(SCK) | GPIO 18 |
|  | DC | GPIO 4 |
|  | CS | GPIO 5 |
|  | RESET | GPIO 16 |
| DHT11 Sensor | VCC | 3.3V |
|  | GND | GND |
|  | DATA | GPIO 13 |
| DS18B20 Sensor | VCC | 3.3V |
|  | GND | GND |
|  | DATA | GPIO 15 |
| PM2.5 Sensor | V-LED | 5V (**through 150ohm resistor & 220uF capacitor)|
|  | LED-GND | GND |
|  | LED Control | GPIO 25 |
|  | S-GND | GND |
|  | Vo(Analog Output) | GPIO 34 |
|  | VCC | 5V |
| MQ135 Sensor | VCC | 5V |
|  | GND | GND |
|  | DO |  |
|  | AO | GPIO 32 (**Voltage Divider with 2x 10 kilo-ohm resistors)|

***Please Note: Make the electrical connections for PM2.5 and MQ135 sensors as shown in pictures in datasheets folder since it includes extra components such as capacitors & resistors*

## 🧠 Software Functionality
✅ Setup
  - Sets designated esp32 gpio pins as inputs/outputs
  - Configures humidity sensor, temperature sensor, MQ135 sensor
  - Configures the oled display to default settings
  - Connects ESP32 to WiFi with given credentials having a retry logic for 30sec to establish a connection
  - Setup ESP32 as a server to listen to requests sent by other devices on the same network
  - Configures time to be fetched from NTP server
    
✅ Sensor Reading Cycle (every 60 seconds)
  - **DHT11**: Reads Humidity
  - **DS18B20**: Reads temperature with retry logic to avoid invalid readings
  - **PM2.5 (GP2Y1010AU0F)**:
    - Activates LED
    - Reads analog voltage
    - Converts voltage into µg/m³
    - Categorizes dust density level
  - **MQ-135 Sensor**:
    - Reads analog voltage
    - Converts voltage into ppm level
    - Categorizes air quality level
      
🖥️ Display Output (OLED)
  - Day, Date, Time
  - Humidity(%)
  - Temperature(°C)
  - PM2.5 Dust Density (µg/m³)
  - Air PPM Level
  - AQI Level (e.g., Clean Air, Moderate, Unhealthy, Hazardous)
    
🌐 Web Server Output
  - Styled HTML web page accessible at `http://<ESP32_IP>` showing all live sensor readings

🖥️ Serial Output Monitor
  - Day, Date, Time
  - Humidity(%)
  - Temperature(°C)
  - PM2.5 Dust Density (µg/m³)
  - PM2.5 Dust Level
  - Air PPM Level
  - AQI Level (e.g., Clean Air, Moderate, Unhealthy, Hazardous)

## 🕓 Time Sync via NTP
- **NTP Server**: ```pool.ntp.org```
- **Timezone**: GMT + 5:30(Indian Standard Time)
- Displays synchronized **date**, **time**, and **day of the week**

## 🧪 Dust Level Mapping Logic
| PM2.5 Range (µg/m³) | Air Quality Level |
|---------------------|-------------------|
| 0 - 12 | 🟢 Clean Air |
| 12.1 - 35.4 | 🟡 Moderate |
| 35.5 - 55.4 | 🟠 Unhealthy for Sensitive Groups |
| 55.5 - 150.4 | 🔴 Unhealthy |
| 150.5 - 250.4 | 🟣 Very Unhealthy |
| 250.5+ | 🟤 Hazardous |

## 🚥 Air Quality Mapping Logic
| Air PPM Level | Air Quality Level |
|---------------------|-------------------|
| 0 - 50.0 | 🟢 Clean Air |
| 50.1 - 100.0 | 🟡 Moderate |
| 100.1 - 150.0 | 🟠 Unhealthy for Sensitive Groups |
| 150.1 - 200.0 | 🔴 Unhealthy |
| 200.1 - 300.0 | 🟣 Very Unhealthy |
| 300.1+ | 🟤 Hazardous |

## ⚙️ MQ135 Sensor Prerequisites & Calibration Instructions
MQ135 air quality/dust detector sensor requires an initial burn-in time of 24-48hrs(brand new sensor). For that purpose, please connect the following mentioned pins of the sensor to arduino UNO, powering UNO through PC and burn in for atleast 24hrs to get stable results.

| Module | Pin Name | Arduino UNO GPIO |
|--------|----------|------------|
| MQ135 Sensor | VCC | 5V |
|  | GND | GND |

Once preheat is complete, perform calibration through arduino UNO to determine RO value of the sensor:
1. Install Arduino IDE and Arduino UNO board support via board manager
2. Make the necessary electrical connections(VCC, GND and analog input)
   
| Module | Pin Name | Arduino GPIO |
|--------|----------|------------|
| MQ135 Sensor | VCC | 5V |
|  | GND | GND |
|  | A0 | A0 (No voltage divider required)|
   
4. Select the correct port and board(Arduino UNO)
5. Upload the code from `sensor_calibration` folder to Arduino UNO
6. Open Serial Monitor -> Get the RO values displayed
7. Open the project code from `src` folder and make the following changes to RO and RL
   
```c
void setup_MQ135()
{
  delay(10000);
  MQ135.setRegressionMethod(1); 
  MQ135.init();

  MQ135.setRL(20); // Look up this value from the datasheet
  MQ135.setR0(28.8); // Punch the RO value received from sensor calibration
  MQ135.setA(110.47);
  MQ135.setB(-2.862);
}
```
8. Once finished, proceed with main project execution



## ⚙️ Project Setup Instructions
1. Install Arduino IDE and ESP32 board support via board manager
2. Make the necessary electrical connections
3. Install required libraries
    - `WiFi.h`
    - `DHT.h`
    - `OneWire.h`
    - `DallasTemperature.h`
    - `AdaFruit_SSD1306.h`
    - `AdaFruit_GFX.h`
    - `MQUnifiedsensor.h`
4. Configure WiFi credentials in the code:
```c
const char* ssid = "Your_SSID";
const char* password = "Your_PASSWORD";
```
5. Select the correct port and board
6. Upload the code from `src` folder to ESP32
7. Open Serial Monitor -> Get the ESP32 IP Address -> Open in Browser(ensure you are on same netowrk as ESP32)

## 📂 File Structure
```
ESP32-Environmental-Monitoring/
|--- sensor_calibration/
|       |--- mq135_sensor_calibrate.ino
|--- Datasheets/
|       |--- ESP32-WROOM-32D_Pinout.png
|       |--- DHT11_Pinout.png
|       |--- Dust_Sensor_Pinout.png
|       |--- OLED_Display_Pinout.png
|       |--- ESP32_DevBoard_Pinout.png
|       |--- Dust_Sensor_Connection.png
|       |--- MQ135_sensor_pinout.png
|       |--- MQ135_sensor_voltagedivider.png
|       |--- DS18B20_Pinout.png
|       |--- DS18B20_Datasheet.pdf
|       |--- ESP32_Datasheet.pdf
|       |--- PM2.5_Datasheet.pdf
|       |--- DHT11_Datasheet.pdf
|       |--- MQ135_datasheet.pdf
|--- images/  
|       |--- OLED_Display.jpeg
|       |--- SerialOutput1.png
|       |--- Setup1.jpeg
|       |--- Setup2.jpeg
|       |--- WebInterface.jpeg
|--- src/  
|       |--- environmental_monitoring_system.ino  
|--- LICENSE  
|--- README.md  
```

## 🛠️ Future Improvements
- Battery powered deployment
- Mobile notification for threshold breach
- Data logging on a server like Raspberry Pi/PC for storage and data analysis
- Integration with IoT cloud based dashboards like Blynk, ThingsBoard, Home Assistant
- Accessing ESP32 webserver from around the world(outside home/local network)

## 📸 Project Setup
<p align="center">
  <img src="https://github.com/28anmol/ESP32-Environmental-Monitoring/blob/main/images/Setup1.jpeg" alt="Setup1" width="800" height="600"/>
</p>

<p align="center">
  <img src="https://github.com/28anmol/ESP32-Environmental-Monitoring/blob/main/images/Setup2.jpeg" alt="Setup2" width="800" height="600"/>
</p>

## 📸 OLED Screen Readings
<p align="center">
  <img src="https://github.com/28anmol/ESP32-Environmental-Monitoring/blob/main/images/OLED_Display.jpeg" alt="OLED Display" width="800" height="600"/>
</p>

## 📸 Web Interface Display
<p align="center">
  <img src="https://github.com/28anmol/ESP32-Environmental-Monitoring/blob/main/images/WebInterface.jpeg" alt="ESP32 Web Interface" width="200" height="400"/>
</p>

## 📸 Serial Monitor Output
<p align="center">
  <img src="https://github.com/28anmol/ESP32-Environmental-Monitoring/blob/main/images/SerialOutput1.png" alt="Serial Monitor Output" width="350" height="200"/>
</p>

## 🙌 Acknowledgements
- Inspired by practical environmental IoT monitoring solutions - targeted for deployment in a household setup
- Thanks to extensive libraries by Adafruit, Dallas Semiconductors

## 📜 License
This project is open souce under MIT License

## 🧑‍💻 Author
🧑‍💻 Anmol Singh  
📍 Pune, India


=======
# IoT-Environmental-Monitoring
>>>>>>> 5b368166a7947efb8a7c1d8f8f701d676fdc231d
