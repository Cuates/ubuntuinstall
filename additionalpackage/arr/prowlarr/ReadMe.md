[*Arr Installation Script](https://wiki.servarr.com/install-script)<br />

* Copy and Paste the ArrInstall.sh script into a local file
  * Visit the *Arr Installation Script for the most up to date script
  * If there are any variable needed to change, then do so now
  * Save and Exit
* Execute command
  * `sudo bash ArrInstall.sh`
* Follow on screen instruction
  * Default should be sufficient
  * **WAIT FOR THIS TO FINISH**
* Add username to group
  * `sudo adduser <username> media`
* Add Port 9696 in Router
* Add Port in [UFW](https://github.com/Cuates/ubuntuinstall/tree/main/system/ufw)
  * `sudo ufw allow 9696/tcp`
* Visit Prowlarr webUI
  * http://<ip_address>:9696
  * http://localhost:9696
* Authentication Required
  * Authentication
    * Choose Forms (Login Page)
  * Authentication Required
    * Choose Enabled
  * Username
  * Password
  * Click button Save
* If using a reverse proxy (Nginx)
  * Modify Prowlarr settings general URL Base in the webUI
    * /prowlarr
