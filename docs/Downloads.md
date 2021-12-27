---
description: Differences between firmware binaries
---

Here you find the recent downloads of my projects.

##ESP32
- [Phc2Mqtt v4.2.0.b]()
The initial version of Phc2Mqtt hardware (v1.2) was the learning school for ESP32 and FreeRTOS development, the firmware used on the module used a partitioning scheme that was not so optimal due to an unused 1Mb factory partition. 

The second generation of Phc2Mqtt hardware (v1.3) was flashed with an improved partitioning scheme, however still using only 4Mb of the flash memory.

Since the used ESP32 WROVER modules all are of subtype E (8Mb of flash) we decided to move to a single partitioning scheme, this requires reflashing the module (no possible via OTA upgrade).

As not all Phc2Mqtt users have the same skills so I needed to look for a solution to re-flashing, in the form of the [WebInstaller](WebInstaller).



So it would be nice to upgrade the first generation o compile your own binary if these pre-compiled builds meet your needs. These available files provide a simpler approach to get up and going with Tasmota quickly.

Release binaries are from the [official OTA server](http://ota.tasmota.com/tasmota/release/). Firmware built from development branch code is available from the [development OTA server](http://ota.tasmota.com/tasmota/).

Features that are not available in any official release build have to be enabled in source code and compiled yourself. Read more about [compiling your own build](Compile-your-build).

!!! tip
    You might find some of the features you need included in one of our unofficial experimental builds over at [https://github.com/tasmota/install](https://github.com/tasmota/install).

## Firmware Variants

- **e32P2m4Mb.bin**  supports ESP32-WROVER-B and ESP32-WROVER-E
- **e32P2m8Mb.bin**  supports ESP32-WROVER-E only

