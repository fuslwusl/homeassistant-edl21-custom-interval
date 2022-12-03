# EDL21

The `edl21` integration lets you read German EDL21 smart meters using [SML](https://de.wikipedia.org/wiki/Smart_Message_Language) from Home Assistant.

In order to connect to the smart meter, an infrared transceiver is required.

Compatible transceivers:

- [DIY](https://wiki.volkszaehler.org/hardware/controllers/ir-schreib-lesekopf-rs232-ausgang)
- [Weidmann Elektronik Schreib-/Lesekopf USB](https://shop.weidmann-elektronik.de/index.php?page=product&info=24)
- [USB IR Lesekopf EHZ Lese-Schreib-Kopf Volkszähler Hichi Smartmeter](https://www.ebay.de/itm/313455434998)

Tested smart meters:

- APATOR Norax 3D (enable InF Mode as described in manual to retrieve full data)
- DZG DWS76 (enable InF as described in manual to retrieve full data)
- Iskraemeco MT175 (ISKRA MT175-D2A51-V22-K0t)
- EMH metering eHZ Generation K (enable InF as described in manual to retrieve full data)
- efr SGM-C4 (enable InF as described in manual to retrieve full data)

## Background

Many community users need a lot quicker scan interval then the forced 60 seconds of the official `edl21` integration. Electical power peaks will not be detected with the standard 60 seconds measurement interval. 
Thanx to @jwefers who listened to the community and added an configuration option `scan_interval_seconds` that allows to set the scan interval down to 1 second. The smart meter delivers values every second but you can set it to whatever you like. Take care of your disk space when using such small intervalls. Recommendation is 10 seconds what is default of this custom integration.
But configurable intervals are not allowed anymore in the official repo (more info here <https://github.com/home-assistant/core/pull/82332/files>) so this was the reason for this custom component.

## Configuration

To set it up, follow this procedure:

1. Copy the folder edl21 to your homeasstant/conf/custom_components/ 
2. Restart homeassistant

   In case you alredy configured the edl21 platform you will get measurements every 10 seconds.
   If you are fine with 10 seconds interval you are finished now.

   In case you want to set another scan interval you just have to add the parameter `scan_interval_seconds` to your `configuration.yaml` file:

   ```yaml
   sensor:
     - platform: edl21
       serial_port: /dev/ttyUSB0
       scan_interval_seconds: 5
   ```

### Configuration variables

**name**
- The friendly name of the smart meter.
- required: false
- type: string

**serial_port**
- The device to communicate with. When using ser2net, use socket://host:port.
- required: true
- type: string

**scan_interval_seconds**
- The interval for measurements.
- required: false
- type: int


## InF Mode

To enable InF mode there are different steps needed based on the meter type but most commonly you have to enter the PIN you received from your grid operator. Once you have it, enter it into the meter and switch to the InF menu where you can switch from InF=Off to InF=On. 
Entering this can be done using a flashlight or (if available) via the physical button on the meter.

For the efr SGM-C4 it is:

- flashing three times to enter pin mode
- entering pin using quicker flashes, wait for 3 seconds for next digit
- pin accepted
- flashing 7 times to get to InF=OFF
- 5-second flash to switch to InF=OFF

You will now get more readings like current Power, Voltage, and phase angle. Some meters don´t have this, in that case only an overall reading is provided.

### ser2net

To use this integration with a remote transceiver you could use [ser2net](https://linux.die.net/man/8/ser2net).

Example `ser2net.conf` configuration file:

> 2001:raw:0:/dev/ttyUSB0:9600 8DATABITS NONE 1STOPBIT
