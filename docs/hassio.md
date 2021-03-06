# Home Assistant setup

## Fix for Home Assistant supervisor not available error:

* Pull new image:  
`docker pull homeassistant/amd64-hassio-supervisor:latest`
* Tag:  
`docker tag homeassistant/amd64-hassio-supervisor:latest homeassistant/amd64-hassio-supervisor:latest`
* Reinstall with sudo access:  
`curl -sL "https://raw.githubusercontent.com/home-assistant/supervised-installer/master/installer.sh" | bash -s -- -m intel-nuc`
* More info about installation [here](https://www.home-assistant.io/hassio/installation/#alternative-install-home-assistant-supervised-on-a-generic-linux-host)


## ZIGBEE2MQTT

* Publish to topic using Developer tools / Services / mqtt.publish service
```yaml
// service data
topic: zigbee2mqtt/bridge/config/permit_join
payload: true
```

* Turn lights off/on via MQTT
```yaml
topic: zigbee2mqtt/kitchen_light_1/set
payload: {"state": "ON"}
```

* [MQTT Client](https://github.com/thomasnordquist/MQTT-Explorer)
* [Pairing Ikea Tradfri bulb example](https://www.zigbee2mqtt.io/devices/LED1836G9.html)

## SmartThings

* Guide [here](https://www.home-assistant.io/integrations/smartthings/)

