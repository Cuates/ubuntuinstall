[How to Install Jellyfin Media Server on Ubuntu 22 04 20 04 Server Desktop](https://www.linuxbabe.com/ubuntu/install-jellyfin-media-server-ubuntu-20-04)<br />
[How to Install Jellyfin Media Server on Ubuntu 22 04 LTS](https://www.linuxcapable.com/how-to-install-jellyfin-media-server-on-ubuntu-22-04-lts)<br />

* Include Default Ubuntu repository
  * `echo "deb [signed-by=/etc/apt/keyrings/jeyllyfin_team.gpg.key arch=$( dpkg --print-architecture )] https://repo.jellyfin.org/ubuntu $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/jellyfin.list`
* Import GPG Key
  * `wget --quiet -O - https://repo.jellyfin.org/jellyfin_team.gpg.key | sudo tee /etc/apt/keyrings/jeyllyfin_team.gpg.key`
* Install Transport and ca certificates
  * `sudo apt-get install -y apt-transport-https ca-certificates`
* Update System with Repository
  * `sudo apt-get update`
* Intall Jellyfin
  * `sudo apt-get install -y jellyfin`
* Jellyfin Status
  * `sudo systemctl status jellyfin`
* Add Port 8096 in Router
* Add Port in [UFW](https://github.com/Cuates/ubuntuinstall/tree/main/system/ufw)
  * `sudo ufw allow 8096/tcp`

* Jellyfin Setup
 * Preferred Display Lanuage
   * English
   * Click Next
 * Username and Password
   * Click Next
 * Wait to add media library
   * Click Next
 * Preferred metadata language
   * English
   * United States
   * Click Next
 * Setup remote access
   * Check box Allow remote connections to this server
   * Click Next
 * Click finish
 * Add Server
   * Connect to Server
     * Host
   * Provide Username and password
   * Click Login
