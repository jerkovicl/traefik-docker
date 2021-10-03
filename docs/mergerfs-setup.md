## MergerFS Disk pooling
* Check disks with command: `sudo parted -l`
* Create mount point for disk: 
   ```
   cd /mnt
   sudo mkdir storage // create dir for mount name
   sudo mount /dev/sda1 /mnt/storage // map partition path to mount path
   df -Th // check results   
   ```
* Create mount point for the union filesystem as a virtual directory: `sudo mkdir virt` inside `/mnt` folder
* Mount the union fs: `mergerfs -o defaults,allow_other,use_ino,fsname=mergerFS /mnt/storage:/mnt/storage2 /mnt/virt/`

> From the mergerFS man page, here’s what the above options do:  
__defaults__: a shortcut for FUSE’s atomic_o_trunc, auto_cache, big_writes, default_permissions, splice_move, splice_read, and splice_write.  
These options seem to provide the best performance.  
__allow_other__: a libfuse option which allows users besides the one which ran mergerfs to see the filesystem. This is required for most use-cases.  
__use_ino__: causes mergerfs to supply file/directory inodes rather than libfuse.  
While not a default it is generally recommended it be enabled so that hard linked files share the same inode value.  
__fsname=name__: sets the name of the filesystem as seen in mount, df, etc. Defaults to a list of the source paths concatenated together with the longest common prefix removed.

* Using command `df -Th` we can see we have a new volume called __mergerFS__, mounted on `/mnt/virt`.
* To persist mergerFS pool upon reboot we will use fstab:
  ```
  # <file system>   <mount point>   <type>         <options>                                      <dump>  <pass>
    /mnt/storage*    /mnt/virt      fuse.mergerfs  defaults,allow_other,use_ino,fsname=mergerFS     0       0
  ```

### Additional info:
* [Docker plex issue](https://github.com/trapexit/mergerfs/issues/463)
* [Docker issues](https://forum.openmediavault.org/index.php?thread/38343-once-and-for-all-should-all-docker-containers-be-stored-outside-of-mergerfs/)
* [Another MergerFS guide](https://zackreed.me/mergerfs-another-good-option-to-pool-your-snapraid-disks/)
* [Move mount point guide](https://help.ubuntu.com/community/MoveMountpointHowto)
* [Mount a partition to two mount points](https://serverfault.com/questions/451223/mount-a-partition-to-two-mount-points)
