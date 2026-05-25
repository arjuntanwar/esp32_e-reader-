# esp32_e-reader-
# ESP32-C3 Smart Watch & E-Reader

A feature-rich smartwatch and e-reader built for the ESP32-C3 Super Mini with OLED display, combining a digital watch with an e-book reader and web-based content management system.

## Features

### ⌚ Digital Watch
- Real-time clock with manual time sync via WiFi
- 24-hour time format with seconds
- Date display (DD/MM) with day of week
- Time persists across deep sleep and power cycles
- Auto-sleep after 30 seconds of inactivity
- Wake on any button press

### 📚 E-Reader
- Read chapters and topics stored in SPIFFS flash memory
- Resume reading from last position
- Smooth scrolling through content
- Automatic text wrapping for 128x64 OLED display
- Support for up to 10 chapters with 20 topics each

### 🌐 Web Content Manager
- Built-in WiFi AP mode for content management
- Complete CRUD operations for chapters and topics
- Rich text editor for topic content
- Responsive web interface (works on mobile/desktop)
- Real-time content updates without re-flashing

### 🔧 Technical Features
- Deep sleep power saving (consumes minimal power when idle)
- RTC memory for persistent time and reading progress
- SPIFFS flash storage (no SD card required)
- Optimized for ESP32-C3 Super Mini

## Hardware Requirements

- **Board:** ESP32-C3 Super Mini
- **Display:** 128x64 OLED (I2C, SSD1306)
- **Buttons:** 4x tactile switches
- **Power:** 3.3V from ESP32-C3

### Pin Connections

| Component | Pin | GPIO |
|-----------|-----|------|
| OLED SDA | 8 | GPIO8 |
| OLED SCL | 9 | GPIO9 |
| OLED Power | 10 | GPIO10 |
| Button UP | 3 | GPIO3 |
| Button DOWN | 4 | GPIO4 |
| Button SELECT | 5 | GPIO5 |
| Button BACK | 6 | GPIO6 |

## Button Controls

| Action | Button Combination |
|--------|-------------------|
| Enter Reader Mode | SELECT + UP |
| Exit to Watch | SELECT + BACK |
| Manual Sleep | UP + BACK |
| Navigate Menus | Single UP/DOWN |
| Select Item | Single SELECT |
| Go Back | Single BACK |

## Reader Mode Menu

1. **Start Reading** - Access your e-books
2. **AP Mode** - Enable WiFi for content management
3. **Update Time** - Sync time via WiFi
4. **Exit** - Return to watch face

## Web Interface

After selecting "AP Mode" in Reader Mode:

1. Connect to **FPV_CAM** WiFi (no password)
2. Open browser to `192.168.4.1`
3. Manage chapters, topics, and content

### Web Features
- ✅ Add/Delete chapters
- ✅ Add/Delete topics  
- ✅ Rename chapters/topics
- ✅ Edit topic content with full text editor
- ✅ Modern dark theme interface

## Installation

### Required Libraries (Install via Arduino Library Manager)

1. **Adafruit GFX** - Display graphics library
2. **Adafruit SSD1306** - OLED driver
3. **ArduinoJson** - JSON parsing for web interface

### Arduino IDE Settings

```
Board: ESP32C3 Dev Module
Flash Size: 4MB (32Mb)
Partition Scheme: Huge APP (3MB No OTA/1MB SPIFFS)
USB CDC On Boot: Enabled
```

### Upload Steps

1. Clone this repository
2. Open the `.ino` file in Arduino IDE
3. Select the correct board and port
4. Click **Upload**

## Code Structure

```
├── Display Functions     - OLED rendering and UI
├── Time Management       - RTC, NTP sync, sleep tracking
├── File Management       - SPIFFS read/write operations
├── Web Server           - AP mode and API endpoints
├── Button Handling      - Input processing and debouncing
└── Main Loop           - State machine and power management
```

## Power Consumption

| Mode | Current Draw |
|------|--------------|
| Active (display on) | ~20mA |
| Light Sleep | ~2-3mA |
| Deep Sleep (planned) | ~10-50µA |

## Time Persistence

The watch uses two mechanisms to maintain accurate time:

1. **RTC Memory** - Unix timestamp stored across reboots
2. **Sleep Duration Tracking** - Uses `esp_timer_get_time()` to measure sleep duration and add to timestamp

This ensures time continues correctly even after deep sleep cycles.

## File Structure on SPIFFS

```
/chapters.json          - Chapter names array
/ch1.json              - Topics for chapter 1
/ch1_1.txt             - Content for chapter 1, topic 1
/ch1_2.txt             - Content for chapter 1, topic 2
...etc
```

## Default Content

On first boot, the system automatically creates:
- 3 default chapters
- 3 topics per chapter with sample content
- All editable via web interface

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Time not syncing | Ensure WiFi credentials are correct and network has internet access |
| Display not working | Check I2C connections and address (default 0x3C) |
| SPIFFS errors | Re-format SPIFFS via Tools > ESP32 Sketch Data Upload |
| Buttons not responding | Check pull-up resistors (internal pull-ups enabled) |
| Sleep not working | Ensure no serial monitor connected (keeps device awake) |

## Customization

### Change Timezone

Modify these lines in the code:
```cpp
const long gmtOffset_sec = 19800;  // IST (UTC+5:30)
const int daylightOffset_sec = 0;
```

### Adjust Sleep Timeout
```cpp
#define WATCH_SLEEP_TIMEOUT_MS 30000  // 30 seconds
```

### Modify Display Brightness
```cpp
int brightness = 200;  // 0-255
```

## Future Enhancements

- [ ] Battery level monitoring
- [ ] Alarm functionality
- [ ] Stopwatch/Timer
- [ ] Bluetooth sync with smartphone
- [ ] OTA updates via web interface
- [ ] Multiple watch face styles

## Credits

- Built with [Adafruit GFX](https://github.com/adafruit/Adafruit-GFX-Library)
- OLED driver by [Adafruit SSD1306](https://github.com/adafruit/Adafruit_SSD1306)
- JSON parsing by [ArduinoJson](https://arduinojson.org/)

## License

MIT License - Free for personal and commercial use

## Author

Developed for ESP32-C3 Super Mini platform

---

**Note:** For best performance, ensure your ESP32-C3 board has at least 4MB flash memory for SPIFFS storage.