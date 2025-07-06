# ValenceRTDisplay_V2

An improved Arduino-based battery monitor for Valence U1-12RT LiFePO4 batteries with corrected RS485 protocol implementation.

## 📖 Overview

This project is an enhanced version of the original [ValenceRTDisplay](https://github.com/Canyoneer/ValenceRTDisplay) by Canyoneer, updated with:

- **Correct 115200 baud rate** (fixed from 9600)
- **Proper Valence RT protocol implementation** based on [ValenceRT-DecodingTool](https://github.com/Canyoneer/ValenceRT-DecodingTool) research
- **Arduino IDE compatibility** (no PlatformIO required)
- **Enhanced display with 4 viewing modes**
- **Cell voltage monitoring**
- **Improved error handling and debugging**
- **Professional color-coded status indicators**

## 🔋 Supported Hardware

- **Batteries**: Valence U1-12RT LiFePO4 batteries (up to 4 units)
- **Display**: TTGO T-Display ESP32 or any TFT_eSPI compatible display
- **Communication**: RS485 TTL converter module
- **Connector**: 5-pin AMP SuperSeal male connector

## 🔌 Wiring Diagram

### TTGO T-Display to RS485 Converter:
```
ESP32 Pin    →    RS485 TTL Converter
GPIO 37      →    TX (transmit to batteries)
GPIO 32      →    RX (receive from batteries)
GPIO 33      →    DE/RE (driver enable, optional)
3.3V         →    VCC
GND          →    GND
```

### RS485 Converter to Valence Battery (5-pin AMP SuperSeal):
```
Converter    →    Battery Pin    →    Function
A+ (RS485+)  →    Pin 3          →    RS485 A
B- (RS485-)  →    Pin 4          →    RS485 B
GND          →    Pin 2          →    Ground
(don't connect)   Pin 5          →    5V Supply
(don't connect)   Pin 1          →    (unused)
```

**Important**: Add a 120Ω termination resistor between RS485 A and B lines.

## 🛠️ Installation

### Prerequisites:
1. **Arduino IDE** (1.8.19+ or 2.x)
2. **ESP32 Board Package** (version 2.0.14 recommended)
3. **TFT_eSPI Library** (version 2.5.x)

### Setup Steps:

1. **Install ESP32 Board Support:**
   - In Arduino IDE: `File` → `Preferences`
   - Add to "Additional Board Manager URLs":
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
   - Go to `Tools` → `Board` → `Board Manager`
   - Search "ESP32" and install "ESP32 by Espressif Systems"

2. **Install TFT_eSPI Library:**
   - Go to `Tools` → `Manage Libraries`
   - Search "TFT_eSPI" and install "TFT_eSPI by Bodmer"

3. **Configure TFT_eSPI for TTGO T-Display:**
   - Navigate to: `C:\Users\[Username]\Documents\Arduino\libraries\TFT_eSPI\`
   - **Backup** `User_Setup_Select.h`
   - Edit `User_Setup_Select.h`:
     ```cpp
     // Comment out:
     // #include <User_Setup.h>
     
     // Uncomment:
     #include <User_Setups/Setup25_TTGO_T_Display.h>
     ```

4. **Load the Project:**
   - Download `ValenceRTDisplay_V2.ino`
   - Create folder called `ValenceRTDisplay_V2`
   - Place the `.ino` file inside the folder
   - Open in Arduino IDE

5. **Configure Board Settings:**
   ```
   Board: ESP32 Dev Module
   Upload Speed: 921600
   CPU Frequency: 240MHz (WiFi/BT)
   Flash Frequency: 80MHz
   Flash Mode: QIO
   Flash Size: 4MB (32Mb)
   Partition Scheme: Default 4MB with spiffs
   PSRAM: Disabled
   ```

## 🎮 Usage

### Button Controls:
- **BTN1 (GPIO 35)**: Manual battery scan/refresh
- **BTN2 (GPIO 0)**: Cycle through display modes

### Display Modes:
1. **Battery Data**: Voltage, current, power, SOC, temperature
2. **Cell Voltages**: Individual cell group voltages
3. **System Info**: Heap memory, uptime, protocol status
4. **Debug Info**: Communication details, battery IDs

### Status Indicators:
- **Green**: Healthy values (voltage >12V, SOC >80%)
- **Yellow**: Caution values (voltage 11.5-12V, SOC 40-80%)
- **Red**: Critical values (voltage <11.5V, SOC <40%)

## 🔍 Protocol Details

### Communication Settings:
- **Baud Rate**: 115,200 (8N1)
- **Protocol**: Custom Valence RT with MODBUS CRC
- **Encoding**: Little endian
- **Update Interval**: 5 seconds

### Protocol Sequence:
1. **Initialization**: Send wake-up sequence
2. **Discovery**: Poll addresses 0-5 for battery IDs
3. **Assignment**: Assign bus IDs to discovered batteries
4. **Data Polling**: Request data from each battery

## 📊 Monitoring Data

### Battery Parameters:
- **Total Voltage**: 0-15V range
- **Current**: ±50A range (+ charging, - discharging)
- **State of Charge**: 0-100%
- **Temperature**: -20°C to +60°C
- **Power**: Calculated from voltage × current
- **Cell Voltages**: Individual cell group readings

### Advanced Features:
- **Dual current readings** (I1, I2 from protocol)
- **Battery identification strings**
- **Real-time protocol debugging**
- **Automatic test data generation** (for development)

## 🔧 Troubleshooting

### Display Issues:
- Verify TFT_eSPI configuration in `User_Setup_Select.h`
- Check ESP32 board package version compatibility
- Test with simple display example first

### Communication Issues:
- Verify 115200 baud rate (not 9600)
- Check RS485 wiring and termination resistor
- Ensure batteries are active (charging/discharging)
- Monitor Serial output for protocol debugging

### No Battery Response:
- Batteries must be active to respond (not idle)
- Check AMP SuperSeal connector pinout
- Verify RS485 polarity (A+ and B- correct)
- Try with batteries under load or charging

## 📈 Development Mode

The code includes **test data generation** for development without real batteries:
- Simulates 4 realistic battery responses
- Allows interface testing and development
- Automatically enables when no real batteries detected

## 🤝 Contributing

This project builds upon excellent work by:
- [Canyoneer](https://github.com/Canyoneer) - Original ValenceRTDisplay and protocol research
- [Bodmer](https://github.com/Bodmer) - TFT_eSPI library
- [LilyGO](https://github.com/Xinyuan-LilyGO) - TTGO T-Display hardware

### Improvements Welcome:
- Additional battery models support
- Enhanced display layouts
- Power optimization
- Alternative hardware platforms

## 📄 License

MIT License - Use, modify, and distribute freely.

## ⚠️ Disclaimer

This is experimental software for educational and monitoring purposes. Always follow proper battery safety procedures and never rely solely on this monitor for critical battery management decisions.

## 🔗 Related Projects

- [Original ValenceRTDisplay](https://github.com/Canyoneer/ValenceRTDisplay) - Original proof of concept
- [ValenceRT-DecodingTool](https://github.com/Canyoneer/ValenceRT-DecodingTool) - Protocol research and Python tools
- [TFT_eSPI](https://github.com/Bodmer/TFT_eSPI) - Display library

---

**Made with ❤️ for the DIY battery monitoring community**
