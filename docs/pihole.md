## Pihole docker setup:

### Modify Ubuntu Network Configuration

```
sudo systemctl disable systemd-resolved.service
sudo systemctl stop systemd-resolved.service
sudo nano /etc/NetworkManager/NetworkManager.conf
```

- Add dns=default under [main] so that the file contents look like what is shown below:
```
[main]
plugins=ifupdown,keyfile
dns=default
...
...
...
```
- Rename resolv.conf file `sudo mv /etc/resolv.conf /etc/resolv.conf.bak`
- In case there is a problem, you can rename the file back to /etc/resolv.conf. Finally, restart your network manager using the following command `sudo service network-manager restart
`
- Accessing PiHole by Commandline with command: `docker exec -ti pihole /bin/bash`
