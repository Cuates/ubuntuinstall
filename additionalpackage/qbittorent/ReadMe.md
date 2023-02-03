[How to Install qBittorrent on Ubuntu 22 04 or 20 04](https://www.linuxcapable.com/how-to-install-qbittorrent-on-ubuntu-linux/)<br />
[How to Install qBittorrent on Ubuntu 19 04 Desktop or Server](https://www.linuxbabe.com/ubuntu/install-qbittorrent-ubuntu-19-04-desktop-server)<br />

* Install Command before packages
  * `sudo apt install -y dirmngr lsb-release ca-certificates software-properties-common apt-transport-https`
* List GPG Keys
  * `sudo gpg --list-keys`
* Import GPG Keys
  * `sudo gpg --no-default-keyring --keyring /usr/share/keyrings/qbittorrent.gpg --keyserver keyserver.ubuntu.com --recv-keys 401E8827DA4E93E44C7D01E6D35164147CA69FC4 > /dev/null`
* Import qBittorent Stable
  * `echo "deb [signed-by=/usr/share/keyrings/qbittorrent.gpg] https://ppa.launchpadcontent.net/qbittorrent-team/qbittorrent-stable/ubuntu $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/qbittorrent.list`
* Update System with new Repository
  * `sudo apt-get update`
* Install qBittorent
  * `sudo apt install -y qbittorrent-nox`
* Create user and group
  * `sudo adduser --system --group qbittorrent-nox`
* Add username to group
  * `sudo adduser <username> qbittorrent-nox`
* Create file /etc/systemd/system/qbittorent-nox.service
  * `sudo vim /etc/systemd/system/qbittorrent-nox.service`
    * Copy and Paste into file
      * <pre>
          [Unit]
          Description=qBittorrent Command Line Client
          After=network.target

          [Service]
          Type=forking
          User=qbittorrent-nox
          Group=qbittorrent-nox
          UMask=007
          ExecStart=/usr/bin/qbittorrent-nox -d --webui-port=8080
          Restart=on-failure

          [Install]
          WantedBy=multi-user.target
        </pre>
  * Save and Exit
* Reload Daemon
  * `sudo systemctl daemon-reload`
* qBittorent Start
  * `sudo systemctl start qbittorrent-nox`
* qBittorent Enable
  * `sudo systemctl enable qbittorrent-nox`
* qBittorent Status
  * `systemctl status qbittorrent-nox`
* Add Port 8080 in Router
* Add Port in [UFW](https://github.com/Cuates/ubuntuinstall/tree/main/system/ufw)
  * `sudo ufw allow 8080`
* Visit qBittorent webUI
  * http://<ip_address>:8080
  * http://localhost:8080
