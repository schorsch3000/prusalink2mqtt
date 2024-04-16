# PrusaLink2MQTT

This script exposes status information from a PrusaLink-Enabled Printer to MQTT.

## Configuration

Configuration is done via environment variables. The following variables are available:

- `MQTT_HOST`: The hostname of the MQTT broker. Mandatory.
- `MQTT_PORT`: The port of the MQTT broker. Default: `1883`.
- `MQTT_USER`: The username for the MQTT broker. Default: None.
- `MQTT_PASS`: The password for the MQTT broker. Default: None.
- `MQTT_PREFIX`: The topic prefix to publish the status to. Default: `PrusaLink/`.
- `PRUSALINK_HOST`: The hostname of the PrusaLink-Enabled Printer. Mandatory.
- `PRUSALINK_APIKEY`: The API key for the PrusaLink-Enabled Printer. Mandatory.
- `POLL_INTERVAL`: The interval in seconds to poll the printer. Default: `15`.
  - Be aware that the http-timeout is set to `POLL_INTERVAL / 3`

## What is does in Detail

The script connects to MQTT and polls the Prusalink API (status, job and info endpoint) every `POLL_INTERVAL` seconds.
All publishes are retained.
It publishes to `MQTT_PREFIX` `Offline` or `Online` depending on the availability of the PrusaLink-API.
If the API-Response gives back data (job does not if the printer is not printing) it publishes a flattend response to
`MQTT_PREFIX/ENDPOINTNAME/FLATTENED/KEY` with the value of the key.
If the key is empty, it deletes all messages under `MQTT_PREFIX/ENDPOINTNAME/`.

## usage

Either run the `run` script on a machine with php-cli >= 8.2 with php-curl and php-mosquitto installed or use the docker image.

There is no storage to configure, everything is stateless, also there is no port to expose, there is no webinterface, it's just a api2mqtt bridge.
