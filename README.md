# ESP32-DEVKIT freewheeling


# Introduction

Get to know this ESP32 v4 evaluation kit (as Server): ![](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/_images/esp32-devkitc-functional-overview.jpg)
Connecting with a HUZZAH32 - ESP32 Feather (as Client): ![](https://cdn-learn.adafruit.com/assets/assets/000/028/690/medium800/adafruit_products_pinbottom.jpg?1448334076)

____

Set up a GATT-server and Client with these devices.  

* Client devices access remote resources over a BLE link using the GATT protocol. Usually, the central is the client (but not necessarily).
* Server devices have a local database and access control methods and provide resources to the remote client. Usually, the peripheral is the server (but not necessarily).
* You can use read, write, notify, or indicate operations to move data between the client and the server.
  * Read and write operations are requested by the client and the server responds (or acknowledges).
  * Notify and indicate operations are enabled by the client but initiated by the server, providing a way to push data to the client.
  * Notifications are unacknowledged, while indications are acknowledged. Notifications are therefore faster but less reliable  

![](https://i2.wp.com/randomnerdtutorials.com/wp-content/uploads/2021/11/BLE-Server-Client-Server-Advertising-03.png?w=750&quality=100&strip=all&ssl=1)

* GATT client - a device which accesses data on the remote GATT server via read, write, notify, or indicate operations
* GATT server - a device which stores data locally and provides data access methods to a remote GATT client
* GATT stands for Generic Attributes and it defines a hierarchical data structure that is exposed to connected BLE devices. This means that GATT defines the way that two BLE devices send and receive standard messages.  
Example -> ![](https://i0.wp.com/randomnerdtutorials.com/wp-content/uploads/2021/11/GATT-ESP32-BLE-Server-Client-Example.png?w=750&quality=100&strip=all&ssl=1)
* <ins>Profile:</ins> standard collection of services for a specific use case
* <ins>Service:</ins> collection of related information, like sensor readings, battery level, heart rate, etc. ; [predefined UUID list](https://btprodspecificationrefs.blob.core.windows.net/assigned-values/16-bit%20UUID%20Numbers%20Document.pdf)
* <ins>Characteristic</ins>: it is where the actual data is saved on the hierarchy (value);
  * <ins>Descriptor:</ins> metadata about the data;
  * <ins>Properties:</ins> describe how the characteristic value can be interacted with. For example: read, write, notify, broadcast, indicate, etc.  
[Link Reference on randomTutorials](https://randomnerdtutorials.com/esp32-ble-server-client/)

## Attribute Protocol (ATT)
The following figure shows a logical representation of an Attribute:
![](https://www.novelbits.io/wp-content/uploads/2017/06/Attribute-structure.be528107546b4df284978c3ab889e0dd-768x123.png)
* Handle: This is a 16-bit value that the server assigns to each of its attributes — think of it as an address.
* Attribute Permissions determine whether an attribute can be read or written
___
## Prerequisite
* [ESP tools](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/tools/index.html)

___
## Useful links
* [hardware ESP32 devkit v4](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/hw-reference/esp32/get-started-devkitc.html)  
  We are not adding any hardware for this test-project but I like to keep the overview at hand.
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

## Gatt Server - Example
* [Gatt Server Example code](https://github.com/espressif/esp-idf/tree/e090b6b0315c0b3219cd55939f3d165457ed2c40/examples/bluetooth/bluedroid/ble/gatt_server)  
Copy the example prog to your local develop
```sh
C:\Users\<name>\esp\gat_server>xcopy /e /i %IDF_PATH%\examples\bluetooth\bluedroid\ble\gatt_server
C:\Users\<name>\esp\gat_server>idf.py build
..[1291]steps..
C:\Users\x\esp\gat_server>idf.py -p COM11 flash
```
### How do we actually flash
* the bootloader goes to address 0x01000  
* the partition table gets on address 0x08000
* the application get on address 0x10000
```sh
C:\Users\x\.espressif\python_env\idf4.4_py3.8_env\Scripts\python.exe d:\esp-idf\components\esptool_py\esptool\esptool.py -p (PORT) -b 460800 --before default_reset --after hard_reset --chip esp32  write_flash --flash_mode dio --flash_size detect --flash_freq 40m 0x1000 build\bootloader\bootloader.bin 0x8000 build\partition_table\partition-table.bin 0x10000 build\gatt_server_demos.bin
```

## Gatt Client - Example
* [Gatt Client Example code](https://github.com/espressif/esp-idf/tree/e090b6b0315c0b3219cd55939f3d165457ed2c40/examples/bluetooth/bluedroid/ble/gatt_client)
* Get the code in place  
```
C:\Users\<name>\esp\gat_client>xcopy /e /i %IDF_PATH%\examples\bluetooth\bluedroid\ble\gatt_client
C:\Users\<name>\esp\gat_client>idf.py build
..[1291]steps..
C:\Users\<name>\esp\gat_client>idf.py -p COM12 flash
```
The Client (Esp32 Feather) will connect on the server (ESP32 Devkit) and we get below output  
**Server output**
```txt
I (7181196) GATTS_DEMO: ESP_GATTS_DISCONNECT_EVT, disconnect reason 0x8
I (7181596) GATTS_DEMO: ESP_GATTS_CONNECT_EVT, conn_id 0, remote 30:ae:a4:1a:27:c6:
I (7181596) GATTS_DEMO: CONNECT_EVT, conn_id 0, remote 30:ae:a4:1a:27:c6:
I (7181756) GATTS_DEMO: update connection params status = 0, min_int = 16, max_int = 32,conn_int = 32,latency = 0, timeout = 400
I (7182956) GATTS_DEMO: ESP_GATTS_MTU_EVT, MTU 500
I (7182956) GATTS_DEMO: ESP_GATTS_MTU_EVT, MTU 500
I (7183116) GATTS_DEMO: GATT_WRITE_EVT, conn_id 0, trans_id 2, handle 43
I (7183116) GATTS_DEMO: GATT_WRITE_EVT, value len 2, value :
I (7183126) GATTS_DEMO: 01 00
I (7183126) GATTS_DEMO: notify enable
I (7183136) GATTS_DEMO: ESP_GATTS_CONF_EVT, status 0 attr_handle 42
I (7183196) GATTS_DEMO: GATT_WRITE_EVT, conn_id 0, trans_id 3, handle 42
I (7183196) GATTS_DEMO: GATT_WRITE_EVT, value len 35, value :
I (7183206) GATTS_DEMO: 00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f
I (7183206) GATTS_DEMO: 10 11 12 13 14 15 16 17 18 19 1a 1b 1c 1d 1e 1f
I (7183216) GATTS_DEMO: 20 21 22
```
**Client output**
```
I (4403) GATTC_DEMO: ESP_GATTC_CONNECT_EVT conn_id 0, if 1
I (4403) GATTC_DEMO: REMOTE BDA:
I (4403) GATTC_DEMO: a8 03 2a eb e7 de
I (4403) GATTC_DEMO: open success
I (4603) GATTC_DEMO: update connection params status = 0, min_int = 16, max_int = 32,conn_int = 32,latency = 0, timeout = 400
I (5723) GATTC_DEMO: discover service complete conn_id 0
I (5723) GATTC_DEMO: SEARCH RES: conn_id = 0 is primary service 1
I (5733) GATTC_DEMO: start handle 40 end handle 43 current handle value 40
I (5733) GATTC_DEMO: service found
I (5743) GATTC_DEMO: UUID16: ff
I (5743) GATTC_DEMO: Get service information from remote device
I (5753) GATTC_DEMO: ESP_GATTC_SEARCH_CMPL_EVT
I (5753) GATTC_DEMO: ESP_GATTC_REG_FOR_NOTIFY_EVT
I (5803) GATTC_DEMO: ESP_GATTC_CFG_MTU_EVT, Status 0, MTU 500, conn_id 0
I (5963) GATTC_DEMO: ESP_GATTC_NOTIFY_EVT, receive notify value:
I (5963) GATTC_DEMO: 00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e
I (5973) GATTC_DEMO: write descr success
I (6043) GATTC_DEMO: write char success
```
____

# Task

The code actually starts with <ins>GAP</ins> [Generic Access Profile](https://learn.adafruit.com/introduction-to-bluetooth-low-energy/gap) , it is what makes your device visible to the outside world, and determines how two device interact. This is low-level compared to GATT.

* [create Universally Unique Identifier/UUID online](https://www.guidgenerator.com/online-guid-generator.aspx)

## Read/write characteristics of an 8 byte array
We can write and read-back values upto 8 byte. Only the READ and WRITE properties were set.
**nRF Connect Screenshot**
![](CharRW8Byte.jpg)
LOG:
```txt
I (15900) GATTS_DEMO: ESP_GATTS_CONNECT_EVT, conn_id 0, remote 54:2f:14:c8:6a:f2:
I (16380) GATTS_DEMO: update connection params status = 0, min_int = 16, max_int = 32,conn_int = 24,latency = 0, timeout = 400
I (16680) GATTS_DEMO: update connection params status = 0, min_int = 0, max_int = 0,conn_int = 6,latency = 0, timeout = 500
I (16940) GATTS_DEMO: update connection params status = 0, min_int = 0, max_int = 0,conn_int = 24,latency = 0, timeout = 400
I (20220) GATTS_DEMO: GATT_READ_EVT, conn_id 0, trans_id 1, handle 42

I (34170) GATTS_DEMO: GATT_WRITE_EVT, conn_id 0, trans_id 2, handle 42
I (34180) GATTS_DEMO: GATT_WRITE_EVT, value len 4, value :
I (34180) GATTS_DEMO: 01 02 03 04
I (38040) GATTS_DEMO: GATT_READ_EVT, conn_id 0, trans_id 3, handle 42

```

## Boot button
The BOOT button is connected to GPIO0 (which is also a bootstrapping pin to set the boot mode), so pressing it will pull GPIO0 low. You can use this as a general purpose button after your firmware is running.  
This GPIO will have a notification trigger with a counter of the amount of boot buttons pressed.


