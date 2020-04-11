# Home Assistant setup

## Fix for Home Assistant supervisor not available error:

* Pull new image:  
`docker pull homeassistant/amd64-hassio-supervisor:latest`
* Tag:  
`docker tag homeassistant/amd64-hassio-supervisor:latest homeassistant/amd64-hassio-supervisor:latest`
* Reinstall with sudo access:  
`curl -sL "https://raw.githubusercontent.com/home-assistant/supervised-installer/master/installer.sh" | bash -s -- -m intel-nuc`
