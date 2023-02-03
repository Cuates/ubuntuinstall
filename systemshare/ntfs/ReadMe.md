* NTFS already installed
* Verify FUSE is installed
  * `ls -lart /lib/modules/5.15.0-58-generic/kernel/fs/ | grep fuse`
* Checking Your Available Partitions
  * To see your devices and their separate file-systems
    * `lsblk`
* Identifying partition with NTFS file system
  * `fdisk -l | grep NTFS`
* Mount NTFS Hard Disk Drive
  * First create a mount point (directory) for the hard disk
    * `sudo mkdir /mnt/windows01`
    * Mount the NTFS hard disk drive
      * `sudo mount -t ntfs-3g /dev/sdc1 /mnt/windows01/`
        * You may get an error message with mounting
          * <pre>
            NTFS signature is missing.
            Failed to mount '/dev/sdc': Invalid argument
            The device '/dev/sdc' doesn't seem to have a valid NTFS.
            Maybe the wrong device is used? Or the whole disk instead of a
            partition (e.g. /dev/sda, not /dev/sda1)? Or the other way around?
            </pre>
          * If so, then make the necessary changes they recommend
        * You may also get a message when mounting
          * <pre>
            The disk contains an unclean file system (0, 0).
            Metadata kept in Windows cache, refused to mount.
            Falling back to read-only mount because the NTFS partition is in an
            unsafe state. Please resume and shutdown Windows fully (no hibernation
            or fast restarting.)
            </pre>
          * The hard disk has been mounted but is in read only mode
* Mount A Hard Disk Drive
  * First create a mount point (directory) for the hard disk
    * `sudo mkdir /mnt/harddrive01`
  * Mount the NTFS hard disk drive
    * `sudo mount /dev/sdc /mnt/harddrive01/`
* Unmount A Hard Disk Drive
  * Make sure to be a directory above the mount point (directory) or anywhere outside the mount point (directory) to execute the umount command
    * `sudo umount /mnt/windows01/` OR `sudo umount /mnt/harddrive01/`

* Additional Information
  * Using Mount Hard Drive for Samba Share File/Folder
    * If mounted, then umount first before proceeding
      * `sudo umount /path/to/mount/point`
    * Manual Mount
      * `sudo mount /path/to/mount/drive /path/to/mount/point -o context="unconfined_u:object_r:samba_share_t:s0"`
    * Etc Fstab Mount
      * Open /etc/fstab
        * `sudo vim /etc/fstab`
          * Modify or paste the following
            * `UUID=<...> /path/to/mount/point auto defaults,context="unconfined_u:object_r:samba_share_t:s0" 0 0`
      * Save and exit
