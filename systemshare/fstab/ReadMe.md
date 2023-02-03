[How To Mount cifs Windows Share On Linux](https://linuxize.com/post/how-to-mount-cifs-windows-share-on-linux/) <br />

* Dependency [cifs-utils](https://github.com/Cuates/ubuntuinstall/tree/main/systemshare/cifs)
* Create the mount shared drive directory
  * `cd /mnt`
  * `sudo mkdir <share_directory_name>`
* Shared drive credenital's file
  * Create credential file
    * `cd /etc`
    * `sudo vim <credential_file_name>`
      * <pre>
          username=username
          password=password
        </pre>
    * Save and Exit
  * Set file permissions for root user only
    * `sudo chown root:root /etc/<credential_file_name>`
      * May not need this if you created as root user
    * `sudo chmod 600 /etc/<credential_file_name>`
* Configure fstab
  * `sudo vim /etc/fstab`
    * <pre>
        # &#60;file system&#62; &#60;dir&#62; &#60;type&#62; &#60;options&#62; &#60;dump&#62; &#60;pass&#62;
        //WIN_SHARE_IP/share_name  /mnt/&#60;share_directory_name&#62;  cifs  credentials=/etc/&#60;credential_file_name&#62;,file_mode=0777,dir_mode=0777 0 0
      </pre>
* Mount share drive
  * `sudo mount /mnt/<share_directory_name>`
* Unmount share drive
  * `sudo umount /mnt/<share_directory_name>`
