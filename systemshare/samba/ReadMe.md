[How to install SAMBA on Ubuntu 22 04 LTS Jammy Linux](https://www.how2shout.com/linux/how-to-install-samba-on-ubuntu-22-04-lts-jammy-linux)<br />

* Install Samba
  * `sudo apt-get install -y samba samba-common samba-client`
* Check status
  * `sudo systemctl status smbd`
* Allow through [UFW](https://github.com/Cuates/ubuntuinstall/tree/main/system/ufw)
  * `sudo ufw status verbose`
  * `sudo ufw allow samba`

### Creating and Sharing Linux File Share
* Delete an Existing Group
  * `sudo groupdel <groupname>`
* Verify Group was Removed
  * `cat /etc/group`
* Remove User from Group
  * `sudo gpasswd -d <username> <groupname>`
* Create a New Group Name for Samba
  * `sudo groupadd <new_group_name>`
* Associate New User Name to New Group Name
  * `sudo useradd <new_user_name> -G <new_group_name>`
* Associate Existing User to Group
  * `sudo usermod -a -G <new_group_name> <existing_user_name>`
* Create New User Name Samba Password
  * `sudo smbpasswd -a <new_user_name>`
    * New SMB password: <new_user_name_samba_password>
* Optional Using Mount Hard Drive
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
* After Successful Mount
  * `sudo chmod -R 0777 /path/to/mount/folder`
    * This will help in permission access to all file(s) and folder(s)
* Create a Secured Folder
  * `sudo mkdir -p /path/to/secured/folder`
* Allow to listen through SELinux
  * `sudo chmod -R 0777 /path/to/secured/folder`
  * `sudo chown -R <user_name>:<group_name> /path/to/secured/folder`
  * To survive system relabel
    * `sudo semanage fcontext -a -t samba_share_t '/path/to/secured/folder(/.*)?'`
    * `sudo restorecon -R -v /path/to/secured/folder`
      * **IMPORTANT NOTE Do not add the -F flag to restorecon as it will change the user, role, range portion as well as the type which we do not want**
      * **NOTE: Do not do chcon below, issue when this is executed on other local computers**
        * `sudo chcon -t samba_share_t -R /path/to/secured/folder`
    * Display the SELinux context for particular file and or directory
      * `ls -lZ`
* Make Backup of Existing Conf File
  * `sudo cp -pf /etc/samba/smb.conf /etc/samba/smb.conf.bak`
* Edit and Save /etc/samba/smb.conf **NOTE: Place the following at the end of the file**
  * **NOTE** The "veto files" are set because Mac OS writes "._*" and ".DS_Store" files while viewing and visiting files and folders
  * `sudo vim /etc/samba/smb.conf`
    * <pre>
      [global]
      workgroup = &lt;Windows_Work_Group&gt;
      security = user
      passdb backend = tdbsam
      printing = cups
      printcap name = cups
      load printers = yes
      cups options = raw
      veto files = /._*/.DS_Store/
      
      [&lt;secured_shared_name&gt;]
      comment = Private Share Drive
      path = /path/to/secured/folder
      browsable = yes
      writable = yes
      read only = no
      available = yes
      public = no
      guest ok = no
      create mask = 0777
      directory mask = 0777
      # force user = &lt;new_user_name&gt;
      # force group = @&lt;new_group_name&gt;
      valid users = @&lt;new_group_name&gt;, &lt;new_user_name&gt;
      # read list = &lt;new_user_name&gt;
      # write list = &lt;new_user_name&gt;
      </pre>
* Restart Samba Services
  * `sudo systemctl restart smb.service nmb.service`
* Check Samba settings
  * `sudo testparm`
    * Press Enter for an overview of smb.conf file

### Accessing Linux File Share Via Windows
* Access the Linux file share
  * Windows Key + r
    * `\\Machine_name_or_IP_Address`
    * `\\hosts_windows_alias_name\samba_remote_alias_name`
  * Browse shared folders
    * Login with username and password from above
* Windows 10 error 1219 that does not allow multiple user connections from windows **NOTE: May not be needed**
  * Windows command prompt
    * `net use`
    * `net use /delete *` OR `net use /delete \\Machine_name_or_IP_Address \path\to\secured\folder`
  * Then re-attempt to access the file share
* Windows modification to connect to multiple Samba shares at once
  * Paste and Modify the following at the end of the C:\Windows\System32\drivers\etc\hosts file
    * <pre>
      &lt;IP_Address&gt; &lt;Alias_Name&gt;
      &lt;IP_Address&gt; &lt;Alias_Name&gt;
      &lt;IP_Address&gt; &lt;Alias_Name&gt;
      </pre>
  * Save and Exit
  * Then attempt to map the share again

### Accessing Linux File Share Via Mac
* To connect to an SMB file server using a different username, you can use this procedure
  * In the Finder, choose the Go menu, then select Connect to Server
  * Type the network address for the computer or server in the Server Address field in the following format
    * `smb://username:*@server_name_ip_address`
      * The "*" is to trigger the server login window for your SMB server, so that the password for the other_username account can be entered
    * Click the Connect button
    * Enter the desired username and password when prompted
      * Username: other_username
      * Password: The current account password for other_username
      * Select the share on your SMB server that you want to use
      * ** WARNING: Do not try to mount the same share twice using different usernames **
  * One way you can verify that you’re actually connected using different usernames is to use the mount command in Terminal. This should show all mounted volumes on the Mac, including mounted fileshares. The fileshare mount information should include which account was used to mount the share
    * Open a terminal
      * `mount`
  * ** NOTE: Depending on your file server, this approach may not work consistently. On our Isilon storage, the SMB share would mount with the user-specified username every time. On another server I tested, the server would prefer the specific username that was last used to connect and keep using that username when mounting additional shares **
