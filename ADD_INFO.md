# homeassistant-goecharger-mqtt additional infos

## Deployment

Copy the `custom_components` folder to your home assistant `custom_components` folder.

## Optimization to mitigate the go-e Chargers MQTT spam

### MQTT

- Use Mosquitto ACLs to avoid accept and publish few topics from the charger
- This only reduce the traffic between MQTT and HA

```text
user <charger>
# hz of the power
topic deny go-eCharger/+/avgfhz
topic deny go-eCharger/+/fhz
# current timestamp
topic deny go-eCharger/+/utc
topic deny go-eCharger/+/loc
# time after reboot
topic deny go-eCharger/+/rbt
# wlan signal
topic deny go-eCharger/+/rssi
```

### Home Assistant

- Use this integration
   - Reduce a little bit the very long decimal places - that I don't really need
- Disable a lot of (diagnose) sensors that's not important for me
   - This has the affect, there will not be subscribed by the integration and therefor be not processed
   - Primarly `nrg` like Power factor L* & N, Voltage L*
   - WiFi `rssi`
- For the most entities exclude them from recording (to save processing and storing)

## Changelog

### 3.1.0 / Oktober 2026

Starting with originals 3.1.0 and do following changes:
- `custom_components/goecharger_mqtt/__init__.py`: because I do not forward the `utc` attribute in MQTT to HA, I changed the _heartbeat_ from `utc` to `nrg` and set the timeout seconds to 300
- `custom_components/goecharger_mqtt/definitions/sensor.py`: changed the decimal place of the floats `nrg` to _0_ or _1_ for the most of the `nrg` values to save space and not need 5 places
