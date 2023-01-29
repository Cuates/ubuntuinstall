* Installing XRDP
  * `sudo apt-get install -y xrdp`
* Check version
  * `xrdp --version`
* Verify that Xrdp is running
  * `sudo systemctl status xrdp`
* If not active and enable, execute the following commands
  * Enable at boot
    * `sudo systemctl enable xrdp`
  * Start Xrdp
    * `sudo systemctl start xrdp`
