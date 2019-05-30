# ESP32 Windows Setup Tutorial
Dan Goodman, S19

## Assumptions
This tutorial assumes the following:
- You have the Arduino IDE installed
- Know how to use the Arduino IDE fairly well
- You have an ESP32 board that you can connect over USB to test with

## Table of contents
1) [Installing the ESP32 board](#installing-the-esp32-board)
2) [Testing With Your Board](#testing-with-your-board)


### Installing the ESP32 board

- Open the Arduino IDE, and click on the top tab: `File > Preferences`
- At the bottom, where the long text box is labeled `Additional Board Manager URLs`, enter the following on a new line:
    `https://dl.espressif.com/dl/package_esp32_index.json`
- Click `OK` and the window will close
- Next open from the top tab: `Tools > Board: ".." > Boards Manager...`
- When it finishes loading, search for `esp32`
- There should be a board package called "**esp32** by **Espressif Systems**", click install in the bottom right of that card to install. The current version as of writing is `1.0.2`

### Testing With Your Board
- With the Arduino IDE open, plug in your board over USB
- Then select `Tools > Board: ".." > ESP32 Dev Module` under the `ESP32` board section
- Then select the correct port by going to `Tools > Port: ".." > [PORT]`
- Then go to `File > Examples > WiFi > WiFiClient`. This will load an example connecting to WiFi from the device. Input your ssid and password for the network (blank or null for password if open network), and it will connect as you can see in the output of the serial monitor

At this point, you can do normal Arduino and C++ web request and libraries to interact with the internet. Good luck!
