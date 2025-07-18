# Bitaxe Monitor Project - Build Guide

Welcome to the Bitaxe Monitor Project! This custom UI allows you to monitor your Bitcoin mining hardware (Bitaxe) on a CYD 2432S028R board using an ESP32 microcontroller. Follow this guide to set up and deploy the project with ease.

---

## What's New in Version 2.1

### WiFiManager and Custom Display Settings
This version introduces user-friendly features for setup and customization:
- **WiFiManager for Easy Configuration**: Configure WiFi credentials, Bitaxe URL, and display rotation via a web portal without modifying the code.
- **Display Rotation**: Adjust the screen rotation (0-3); 2 is the standard rotation for USB port on the right.
- **Turnoff Status LED**: Option to turn off the Status Led at the back
- **"Powersaving"** Mode: Option to turn off the Display after 1min / 5min / 30min / never
- **Price Data for several mineable Coins**: Choose from BTC, BCH, DGB, XEC, NMC, PPC and LCC
- **Reset Function**: Reset all settings by long-pressing (10 seconds) anywhere on Layouts 1, 2, or 3 to return to the configuration portal.
- **Precompiled Firmware**: Download a single `.bin` file to flash the project easily using a web-based tool—no Arduino IDE required.
- **Multi Device Support**: `Bitaxe_Monitor_Multi_Device.bin` let you add up to 5 Bitaxe devices. Every 30sec the Display changes to the next Bitaxe.

---

## What's New in Version 2.0

### Swipe Functionality and Multiple Layouts
This updated version supports **three different layouts**:
- Change the layout by **swiping from the middle of the screen to the right**.
- Go back to the previous layout by **swiping from the middle to the left**.

### Achievements Feature
- Unlock achievements and see a **small celebration screen** (close by tapping it).
- Access the **achievements page** by swiping right from Layout 3 (go back by swiping left).
- **Press and hold** an achievement to see a brief explanation of why you unlocked it.

Can you unlock every achievement?! (I hope not; there’s an overheating one! xD)

#### Resetting Achievements
If you want to start fresh:
1. download the `Reset_Achievements.bin` and install it via https://web.esphome.io/
   
or

1. Download the sketch `Reset_Achievements.ino` and place it in your Arduino sketches folder.
2. Open the sketch in the Arduino IDE (it will create a folder for it).
3. Select `ESP32 DEV Module` and the correct `COM` port, then upload the sketch.
4. After uploading, re-upload the `Bitaxe_Monitor` sketch.
5. Your achievements will now be reset.

---

## Getting Started: Choose Your Setup Option

You can set up the Bitaxe Monitor in two ways:
- **Option 1**: Flash a precompiled firmware file (recommended for beginners, no coding required).
- **Option 2**: Compile and upload the code manually using the Arduino IDE (for advanced users or customization).

### Prerequisites
Before starting, ensure you have:
- **Hardware**:
  - CYD 2432S028R board (ESP32-based with a 320x240 TFT display)
  - USB cable
  - Bitaxe mining hardware (connected to your local network)
- **Software (for Option 1 - Precompiled Firmware)**:
  - A modern browser like Chrome, Edge, or Opera (for web-based flashing tools)
- **Software (for Option 2 - Manual Compilation)**:
  - Arduino IDE
- **Network**:
  - Wi-Fi credentials
  - IP address of your Bitaxe device on your local network

---

## Option 1: Flash Precompiled Firmware (Recommended for Beginners)

This is the easiest way to get started. Flash the firmware directly to your ESP32 using a web-based tool without installing any software.

### Step 1: Download the Firmware
- Download the precompiled firmware file `Bitaxe_Monitor_Web.bin` or `Bitaxe_Monitor_Multi_Device.bin`

### Step 2: Flash the Firmware Using ESP Web Tools
1. Connect your CYD 2432S028R board to your computer via USB.
2. Open Chrome, Edge, or Opera and visit the website: https://web.esphome.io/
3. Click on "Connect" and select the COM port of your ESP32. (Check the Device Manager. If needed, install the CP210x driver.)
4. Click "Install".
5. Click "Choose File" and upload `Bitaxe_Monitor_Web.bin` / `Bitaxe_Monitor_Multi_Device.bin`. Now click Install.
6. On the backside of your CYD are 2 buttons on the left side of the ESP32 Chip (BOOT & RST). Hold `Boot` (after pressing Install), click `RST`, and release `Boot`.
7. Wait until the flashing is complete.

### Step 3: Configure Settings via WiFiManager
1. After flashing, the ESP32 will restart and create a Wi-Fi Access Point named **"BitaxeMonitorAP"** with the password **"password"**.
2. Connect to this network with your phone or computer.
3. Open a browser and navigate to `192.168.4.1` to access the WiFiManager portal.
4. Enter the following settings:
   - **WiFi SSID**: Your Wi-Fi network name.
   - **WiFi Password**: Your Wi-Fi password.
   - **Multi Device**: If you chose the Multi Device File, choose how many Bitaxe you want to cover. (upto 5)
   - **Bitaxe URL**: The IP address of your Bitaxe device (e.g., `192.168.1.100`).
   - **Display Rotation (0-3)**: Standard is Rotation 2. Only change if your display looks scrambled or you want to place your display differently (e.g., 2 for USB right, 0 for USB left).
   - **Powersaving**: Choose if you want your Display to turn off after some time (never is standard)
   - **Status Led**: Choose if you want to turn it off or leave it on.
   - **Price Data and Blk/Yr***: Choose from BTC, BCH, DGB, XEC, NMC, PPC or LCC 
5. Click "Save". The ESP32 will restart and connect to your Wi-Fi network using the provided settings.

### Step 4: Reset Settings if Needed
- If you need to change settings (e.g., WiFi, Bitaxe URL, or rotation), **long-press (10 seconds)** anywhere on Layouts 1, 2, or 3 of the display. The ESP32 will restart and reopen the "BitaxeMonitorAP" for reconfiguration.

---

## Option 2: Compile and Upload Using Arduino IDE (Advanced Users)

For those who prefer to compile the code themselves or make custom modifications, follow these steps.

### Step 1: Set Up the Arduino IDE
1. **Install ESP32 Board Support**:
   - Open the Arduino IDE.
   - Go to `Tools` > `Board` > `Boards Manager`, search for `ESP32`, and install the package by Espressif Systems.
2. **Select Board and Configure Settings**:
   - Under `Tools` > `Board` > `ESP32`, select `ESP32 Dev Module`.
   - Select the correct COM port.
   - Under `Tools`, configure:
     - `PSRAM` to `Enabled`
     - `Partition Scheme` to `Huge APP (3MB No OTA / 1MB SPIFFS)`

### Step 2: Install Required Libraries
1. Go to `Sketch` > `Include Library` > `Manage Libraries...`.
2. Search for and install the following libraries:
   - `TFT_eSPI` by Bodmer
   - `lvgl` by kisvegabor (version 8.3.11)
   - `WiFi` (usually included)
   - `HTTPClient` (usually included)
   - `ArduinoJson` by Benoit Blanchon
   - `XPT2046_Touchscreen` by Paul Stoffregen
   - `Preferences` (usually included)
   - `SPI` (usually included)
   - `WiFiManager` by tzapu

### Step 3: Download and Open the Project
1. **Download the Project Files**:
   - Download the `Bitaxe_Monitor.ino`, `background.c`, `background.h`, `User_Setup.h`, `lv_conf.h`, `background2.c`, `background2.h`, `background3.c`, `background3.h`, `backgroundachieve.c`, `backgroundachieve.h`, `achievementpopup.c`, and `achievementpopup.h` files. (`reset_achievements` is optional)
2. **Place Configuration Files in Correct Folders**:
   - Place `User_Setup.h` in the `TFT_eSPI` library folder (`Documents\Arduino\libraries\TFT_eSPI`).
   - Place `lv_conf.h` in the libraries folder (`Documents\Arduino\libraries\`).
   - Keep all other files in the same folder as the `.ino` file (e.g., `Documents\Arduino\Bitaxe_Monitor`).
3. **Open the Sketch**:
   - In the Arduino IDE, go to `File` > `Open` and select the `Bitaxe_Monitor.ino` file.

### Step 4: Compile and Upload the Sketch
1. Connect your CYD 2432S028R board via USB.
2. In the Arduino IDE, click `Sketch` > `Verify/Compile` to check for errors.
3. If successful, click `Sketch` > `Upload` to upload the code to your board. If necessary, hold the `BOOT` button, then press `RESET`, and release the `BOOT` button on the board during upload.

### Step 5: Configure Settings via WiFiManager
Follow the same steps as in Option 1, Step 3 to configure WiFi, Bitaxe URL, and display rotation via the WiFiManager portal.

---

## Troubleshooting

| **Issue**                       | **Solution**                                                                                           |
|---------------------------------|-------------------------------------------------------------------------------------------------------|
| **Compilation Errors (Option 2)** | Ensure all required libraries are installed. Check if `User_Setup.h` is in the `TFT_eSPI` folder, `lv_conf.h` in the libraries folder, and all `backgroundXXX.c/.h` in the sketch folder. |
| **Wi-Fi Connection Issues**     | Verify that the SSID and password are entered correctly in the WiFiManager portal. Ensure the board is within range of your Wi-Fi network. |
| **No Data from Bitaxe**         | Confirm that the Bitaxe URL (IP address) in the WiFiManager portal is correct and the device is online and accessible on your network. |
| **No COM Port Visible**         | Check the Device Manager. If needed, install the CP210x driver.                                       |
| **Performance/Memory Issues (Option 2)** | Set PSRAM to `Enabled` and Partition Scheme to `HUGE APP`.                                    |
| **Display Issues**              | - **Rotated Display**: Adjust the „Display Rotation“ value (0-3) in the WiFiManager portal (`192.168.4.1`).<br>- **Incorrect Colors**: Refer to the „Advanced Configuration“ section below for instructions on adjusting color settings.<br>- **Access Portal**: Long-press (10 seconds) on Layouts 1, 2, or 3 to reset settings and reopen the portal. |

---

## Advanced Configuration: Fixing Color Issues

If you experience incorrect colors on your display (e.g., blue instead of red/orange), this is often due to hardware differences in the display driver of the CYD 2432S028R board. You can fix this by adjusting the color order in the configuration file:

1. **Locate the `User_Setup.h` File**:
   - Find `User_Setup.h` in your Arduino libraries folder under `Documents\Arduino\libraries\TFT_eSPI`.
2. **Edit the Color Order**:
   - Open `User_Setup.h` in a text editor.
   - Search for the line defining the color order (e.g., `#define TFT_RGB_ORDER TFT_RGB` or `#define TFT_RGB_ORDER TFT_BGR`).
   - Switch between `TFT_RGB` and `TFT_BGR` by uncommenting one (remove the // on one of them.):
     ```
     // #define TFT_RGB_ORDER TFT_RGB  // Colour order Red-Green-Blue
     // #define TFT_RGB_ORDER TFT_BGR  // Colour order Blue-Green-Red
     ```

3. **Restart Ardunio**
4. **Recompile and Upload**:
   - Save the file, recompile your sketch in the Arduino IDE, and upload it to your ESP32 board.
5. **Test the Display**:
   - Check if the colors are now displayed correctly. If not, try the other setting.

---
## Bugs
   - Some Achievements possibly bugged for `Bitaxe_Monitor_Multi_Device`

**Note**: This is an advanced configuration step and requires recompiling the firmware. It is recommended for users familiar with Arduino programming.

---

***Note on Block Chance Calculations (`Blk/Yr`)**:  
The `Blk/Yr` value represents an estimated probability of mining a block per year based on your device's hashrate compared to the network's total hashrate. For Bitcoin (BTC), this calculation uses real-time network data and is accurate. For other coins (BCH, XEC, DGB, NMC, PPC, LCC), the values are approximations based on static adjustment factors relative to Bitcoin's difficulty. These approximations may not reflect current network conditions and should be considered rough estimates. I am working on improving accuracy for non-BTC coins in future updates.

---
## License

This project is open for personal and educational use. If you redistribute or modify it, please credit the original author.
