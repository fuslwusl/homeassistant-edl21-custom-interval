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
The smart meter delivers values every second but to avoid instabilities this version is set to a fixed interval of 6 seconds.


## Custom interval

If you want to set another interval you have to make a change in the file `sensory.py` in something at line 44:

```python
MIN_TIME_BETWEEN_UPDATES = timedelta(seconds=6)
```

Just change the value from seconds=6 to what you like. 
But is is recommendet to set the value not lower than 5 seconds!

Take care that every update of this edl21-custom-interval integration will overwrite your custom interval.


**WARNING:** This version uses a new UI configuration flow and could contain some breaking changes.

* Take care of your historic data and make a backup first.
* Try the update on a test system first if possible.
* Make a full system backup of Home assistant so you can restore if something goes wrong.

## Migration
* Remove the yaml configuration file for edl21
* Installing this new version with the UI configuration
* Restart Homeassistant - the migration process should start and you should get data every 6 seconds

If something failes you can restore your backup.
