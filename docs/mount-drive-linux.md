## Mount drive in linux and set auto-mount at boot

* Mount drive:
```
cd /mnt
sudo mkdir /media/data
sudo mount /dev/sdb1 /media/data
```
* Auto-mount at boot
```
# find drive UUID
ls -al /dev/disk/by-uuid/
# edit fstab file
sudo nano /etc/fstab
# add an entry for the UUID and mount point

# data drive
UUID=19fa40a3-fd17-412f-9063-a29ca0e75f93 /media/data   ext4    defaults        0       0
```
* test fstab mounting: `findmnt --verify`
* Unmounting drive with umount `sudo umount /media/data`
