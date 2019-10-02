## Redis docker setup:

### Fix THP issues

```
sudo -i
echo never > /sys/kernel/mm/transparent_hugepage/enabled
exit
sudo sysctl vm.overcommit_memory=1
```

- Add this to rc.local file to persist changes after reboot
- Ubuntu 18.04 doesn't contain rc.local file so we need to create it `sudo nano /etc/rc.local`
- paste the following:

```
#!/bin/sh -e
# 
# rc.local
# 
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.
#
echo never > /sys/kernel/mm/transparent_hugepage/enabled
sysctl vm.overcommit_memory=1
exit 0
```
- Save and exit
- Now make the file executable `sudo chmod +x /etc/rc.local`

### Customize config - OPTIONAL

```
mkdir -p $USERDIR/docker/redis/config
docker run --rm --entrypoint cat redis /usr/local/etc/redis/redis.conf > $USERDIR/docker/redis/config/redis.conf
// Mount to /usr/local/etc/redis/redis.conf
```
