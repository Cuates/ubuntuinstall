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
* Add Port 8787 in Router
* Add Port in [UFW](https://github.com/Cuates/ubuntuinstall/tree/main/system/ufw)
  * `sudo ufw allow 8787/tcp`
* Visit Prowlarr webUI
  * http://<ip_address>:8787
  * http://localhost:8787
