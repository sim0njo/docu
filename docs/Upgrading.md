description: Easily upgrade Tasmota to a newer version or different build while keeping all your settings

<span style="font-size:24px;font-weight:bold">The first rule of upgrading: If it ain't broke, don't fix it!</span>

In other words, ensure that there is a good reason to mess with a working installation (e.g., a need to use a new feature or address a found problem fixed in the current version).

Phc2Mqtt provides 2 ways of upgrading.

## Upgrade by file upload

Upgrading the device firmware [over-the-air](https://en.wikipedia.org/wiki/Over-the-air_programming), aka OTA, is the most convenient way to upgrade. 

To start the upgrade, open a web browser to your device's IP-address, you will get the main menu. 

<p align="center">
<img src="../img/main-menu.jpg"></td>
</p>

Now select **Firmware Upgrade**.
<p align="center">
<img src="../img/fwupgrade.jpg"></td>
</p>

Select a firmware file suitable for your module and press **Start Upgrade**


## Upgrade via serial connection

Upgrade over the serial connection using serial-to-USB adapter.

### Flash new firmware without erasing


###Flash new firmware with erasing

Upload the new version over serial using the same process as in [Flashing](Getting-Started#flashing) but DO NOT erase flash. The new binary will overwrite the old one and keep your settings.

