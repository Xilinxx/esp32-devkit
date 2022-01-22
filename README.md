# esp32-devkit


# Introduction

Getting to know this ESP32 v4 evaluation kit ![](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/_images/esp32-devkitc-functional-overview.jpg)
Connecting with a HUZZAH32 - ESP32 Feather ![](https://cdn-learn.adafruit.com/assets/assets/000/028/690/medium800/adafruit_products_pinbottom.jpg?1448334076)

## prerequisite
* [ESP tools](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/tools/index.html)

## Useful links
* [hardware ESP32 devkit v4](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/hw-reference/esp32/get-started-devkitc.html)
![](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/_images/esp32-devkitC-v4-pinout.png)
* [GATT - Generic Attribute Profile](https://learn.adafruit.com/introduction-to-bluetooth-low-energy/gatt)
* [BLE - fundamentals](https://www.embedded.com/bluetooth-low-energy-ble-fundamentals/)

# Getting started

One can start with the *hello world* example [here](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html)  
If all went well the last step will be
```sh
Project build complete. To flash, run this command:
C:\Users\davth\.espressif\python_env\idf4.4_py3.8_env\Scripts\python.exe ..\..\components\esptool_py\esptool\esptool.py -p (PORT) -b 460800 --before default_reset --after hard_reset --chip esp32  write_flash --flash_mode dio --flash_size detect --flash_freq 40m 0x1000 build\bootloader\bootloader.bin 0x8000 build\partition_table\partition-table.bin 0x10000 build\hello_world.bin
or run 'idf.py -p (PORT) flash'
```

## serial connection
![](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/_images/putty-settings-windows.png)

The Hello world output on your terminal should look like
```txt
Hello world!
This is esp32 chip with 2 CPU core(s), WiFi/BT/BLE, silicon revision 3, 2MB external flash
Minimum free heap size: 293260 bytes
Restarting in 10 seconds...
Restarting in 9 seconds...
```
