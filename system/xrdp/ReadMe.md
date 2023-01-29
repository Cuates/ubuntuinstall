* Note: If ufw is not enable, then enable ufw
  * `sudo ufw enable`
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
* Configuring Xrdp
  * The configuration files are located in the /etc/xrdp directory
  * The main configuration file is named xrdp.ini. This file is divided into sections and allows you to set global configuration settings such as security and listening addresses and create different xrdp login sessions.
  * Create a backup file before modifying
  * Modify /etc/xrdp/xrdp.ini file
    * `sudo vim /etc/xrdp/xrdp.ini`
    * Optional
      * Change port to another port number if needed
    * Add the following lines at the end of the file
      * <pre>
          ; XRDP
          exec gnome-session
        </pre>
    * Save and Exit
  * Restart whenever changes are made to xrdp.ini
    * `sudo systemctl restart xrdp`
      * Xrdp uses startwm.sh file to launch the X session. If you want to use another X Window desktop, edit this file.
 * Configure Firewall
   * By default, Xrdp listens on port 3389 on all interfaces
   * Change to custom port number if different from 3389
   * To allow traffic to port 3389 from anywhere use the commands below. Allowing access from anywhere is highly discouraged for security reasons.
     * `less /etc/services`
     * `sudo ufw allow 3389/tcp`
    * **IMPORTANT NOTE For increased security, you may consider setting up Xrdp to listen only on localhost and creating an SSH tunnel that securely forwards traffic from your local machine on port 3389 to the server on the same port. Another secure option is to install OpenVPN and connect to the Xrdp server through the private network.**
* Connecting to the Xrdp Server
  * You can now start interacting with the remote desktop from your local machine using your keyboard and mouse.
  * If you are using macOS, you can install the Microsoft Remote Desktop application from the Mac App Store. Linux users can use an RDP client such as Remmina or Vinagre. Windows can use the Remote Desktop Connection (RDP) which is already installed on Windows.
  * Launch your remote desktop of choice
  * Put in the IP address of the Linux server
  * Input username and password
  * You may get a message for "Authenticate Required to access the PC/SC daemon"
    * Input password for the user currently trying to remote into the system
    * Click Authenticate
