# ESP32-LoRa-Node
<i>(The full CAD project is online at https://easyeda.com/aizukanne/smartnode-esp32.)</i>

<p align="center">
  <img src="https://github.com/aizukanne/ESP32-LoRa-Node/blob/master/PCB%20Files/PCB_ESP32-Board-PCB_20190806110103.png" alt="PCB Layout"/>
</p>

This is a LoRa node, powered by the ESP32 WROVER-B microcontroller. ESP32-WROVER-B is a powerful, generic WiFi-BT-BLE MCU module that targets a wide variety of applications, ranging from low-power sensor networks to the most demanding tasks, such as voice encoding, music streaming and MP3 decoding. ESP32-WROVER-B features a 4 MB external SPI flash and an additional 8 MB SPI Pseudo static RAM (PSRAM). The LoRa module on this board is the RFM95W. The design has an onboard 868MHz antenna with 5dBi gain. More about this in the antenna section below. There is an external 32.768 kHz crystal connected between GPIO32 and GPIO33 to act as RTC sleep clock. The board is designed to be used for production ready solutions where code has been tested and verified rather than where you have to frequently upload new code. However if using Micropython. this will not be an issue.

**Header And Pins:**
A 24-pin header with 2.54mm pitch is provided for external connections. This also allows modules designed specifically for this board to be plugged on to the header. A module can provide 5-V power to the board via pin 23 & 24 (both are 5V inputs) or take 3.3V power from the board via pins 1 & 3. GND is on pins 2 & 4. All internally pins connected e.g. to internal flash, RTC clock crystal or the on-board Lora Module are not exposed in the 24-pin header. Modules installed on the header are stackable as long as no two modules contend for any of the IO pins. However multiple modules can sit on the I2C bus. Sockets with long pins can be used on a module to make it possible for another module to be mounted on top. The Pinout of the 24-pin header is below.

<p align="center">
  <img src="https://github.com/aizukanne/ESP32-LoRa-Node/blob/master/24-PIN%20Header.jpg" alt="24-pin Header"/>
</p>

**Programming:**
This board does not contain a USB-UART interface. For programming or other USB connectivity, a seperate module with a USB-UART bridge controller should be used. Design documents (Schematic and PCB) are in this project [https://easyeda.com/aizukanne/smartnode-esp32-programmer] (ESP32 LoRa Node Programmer). This can be connected to the 24-pin header to program the controller.

The board can be programmed using arduino IDE or micropython. I have tried without success to get pycom's version of Micropython to run on this but for some reason, firmware in flash gets corrupted once the board is power cycled. The latest version of [http://micropython.org/download#esp32] (micropython with SPIRAM support here) works perfectly. However, if you wish to take advantage of the REPL without having the 24-pin header occupied by the USB-UART module, you can use [https://github.com/loboris/MicroPython_ESP32_psRAM_LoBo] (this Micropython) by Boris Lovosevic (Loboris). It provides Telnet and FTP access. This make sit possible to disconnect the USB-UART bridge module and connect to the board via telnet over WiFi. Files can be uploaded using ftp.

**On-board Communication:**
While the ESP32 module provides WiFi and Bluetooth Low Energy (BLE), there is an RFM95W LoRa module on the board. There is also an integrated antenna connected to this module through a matching LC network. The antenna is a 5dBi PCB antenna designed to Texas Instruments design note DN024 specification for 868MHz only. We have been able to achieve 5.99KM with this antenna in an urban area, where a stub antenna could only perform up to 1.5KM. Interrupt pins DIO0, DIO1 and DIO3 are all connected to GPIO23. The pin connections are as follows:

- RADIO_MOSI------------GPIO27
- RADIO_MISO------------GPIO19
- RADIO_SCLK------------GPIO5
- RADIO_NSS-------------GPIO18
- RADIO_DIOO------------GPIO23
- RADIO_DIO1------------GPIO23
- RADIO_DIO2------------GPIO23 

**Power:**
The board can be powered by external 5V input as described above. There is a socket for a 1S LiPo battery with onboard charging. Some battery connectors have their terminals swapped so the positive terminal is marked beside the connector. There is an onboard schottky diode to protect the board if terminals are swapped.
