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
3) [Example AdafruitIO Usage](#example-adafruitio-usage)


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

### Example AdafruitIO Usage

Getting up and going with AdafruitIO is easy. Here are some tips to doing so with an ESP:

1) You will need to include the following libraries after you have installed them:
```c
#include "Adafruit_MQTT.h"
#include "Adafruit_MQTT_Client.h"
```
2) You will want to setup variables like the following:
```c++
// WiFi parameters
#define WLAN_SSID       "SSID"
#define WLAN_PASS       "PASS"
 
// Adafruit IO
#define AIO_SERVER      "io.adafruit.com"
#define AIO_SERVERPORT  1883
#define AIO_USERNAME    "USR"
#define AIO_KEY         "KEY"
WiFiClient client;
```
3) Setup the feeds you want to publish to:
```c++
Adafruit_MQTT_Client mqtt(&client, AIO_SERVER, AIO_SERVERPORT, AIO_USERNAME, AIO_KEY);
Adafruit_MQTT_Publish ex1Feed = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/example1");
Adafruit_MQTT_Publish ex2Feed = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/example2");
```
4) You will want some easy to use connect function:
_hint: the F function inside of the println makes it print straight to stdout without storing in memory at all_
```c++
void connect() {

  Serial.print(F("Connecting to Adafruit IO... "));

  int8_t ret;

  while ((ret = mqtt.connect()) != 0) {
    Serial.println(ret);
    switch (ret) {
      case 1: Serial.println(F("Wrong protocol")); break;
      case 2: Serial.println(F("ID rejected")); break;
      case 3: Serial.println(F("Server unavail")); break;
      case 4: Serial.println(F("Bad user/pass")); break;
      case 5: Serial.println(F("Not authed")); break;
      case 6: Serial.println(F("Failed to subscribe")); break;
      default: Serial.println(F("Connection failed")); break;
    }

    if(ret >= 0)
      mqtt.disconnect();

    Serial.println(F("Retrying connection..."));
    delay(5000);

  }

  Serial.println(F("Adafruit IO Connected!"));

}
```
5) Publish some data to those feeds:
```c++
ex1Feed.publish(376);
ex2Feed.publish(9.44736);
```

As you can see it's quite easy to get going with AdafruitIO on your ESP.
