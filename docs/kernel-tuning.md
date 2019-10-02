## Kernel tuning for docker setup:

### Fix net.core.*mem issues 

- add these to the bottom of `/etc/sysctl.conf`

```
# Allow a 25MB UDP receive buffer for JGroups
net.core.rmem_max = 26214400
# Allow a 1MB UDP send buffer for JGroups
net.core.wmem_max = 1048576
```

- or

```
sudo sysctl -w net.core.rmem_max=26214400
sudo sysctl -w net.core.wmem_max=1048576
// Type the following command to reload settings from all system config files without rebooting the box:
sysctl --system
// or
sysctl -p
```

- Reference info [here](https://www.cyberciti.biz/faq/reload-sysctl-conf-on-linux-using-sysctl/)

### Fix Vscode file limit 
- add these to the bottom of `/etc/sysctl.conf`

```
fs.inotify.max_user_watches = 524288
```
