# homeassistant-goecharger-mqtt additional infos

## Oktober 2026

Starting with originals 3.1.0 and do following changes:
- `custom_components/goecharger_mqtt/__init__.py`: because I do not forward the `utc` attribute in MQTT to HA, I changed the _heartbeat_ from `utc` to `nrg` and set the timeout seconds to 300
- `custom_components/goecharger_mqtt/definitions/sensor.py`: changed the decimal place of the floats `nrg` to _0_ or _1_ for the most of the `nrg` values to save space and not need 5 places
