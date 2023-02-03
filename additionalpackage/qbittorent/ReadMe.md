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

* Set up the setting for qBittorrent
  * **These are based on Windows application settings**
  * Behavior
    * Interface
      * Language
        * English
    * Transfer List
      * Check
        * Confirm when deleting torrents
        * Use alternating row colors
    * Desktop
      * Check
        * Confirmation on auto-exit when downloads finish
          * Show qBittorrent in notification area
          * Minimize qBittorrent in notification area
          * Close qBittorrent to notification area
          * Tray icon style
            * Select
              * Monochrome (for dark theme)
          * File association
            * Use qBittorrent for .torrent file
            * Use qBittorrent for magnet links
        * Check for program update
    * Power Management
      * Check
        * Inhibit system sleep when torrents are downloading
        * Inhibit system sleep when torrents are seeding 
    * Log File
      * Check
        * Backup the log file after 65 KiB
        * Delete backup logs folder older than 1 months
  * Downloads
    * When adding a torrent
      * Check
        * Display torrent content and some options
        * Bring torrent dialog to the front
    * Check
      * Enable recursive download dialog
    * Saving Management
      * Default Save Path
        * Browse
          * <path_to_torrent_directory>
  * Connections
    * Peer Connection Protocol
      * Select
        * TCP and muTP
    * Listening Port
      * Port used for incoming connections
        * Choose any port you like or leave as default
      * Check
        * Use UPnP / NAT-PMP port forwarding from my router
    * Connection Limits
      * Check
        * Global maximum number of connections
          * Choose number of connection
            * i.e. 500
            * **Note** If you set this value higher than your connection speed may vary
        * Global maximum number of connections per torrent
          * Choose number of connection
            * i.e. 100
            * **Note** If you set this value higher than your connection speed may vary
        * Global maximum number of upload slots
          * Choose number of connection
            * i.e. 1
            * **Note** If you set this value higher than your connection speed may vary
        * Global maximum number of upload slots per torrent
          * Choose number of connection
            * i.e. 1
            * **Note** If you set this value higher than your connection speed may vary
  * Speed
    * Global Rate Limits
      * Upload
        * Select
          * i.e. 10 KiB/s
      * Download
        * Select
          * i.e. (infinity)
    * Alternative Rate Limits
      * Upload
        * Select
          * i.e. 10 KiB/s
      * Download
        * Select
          * i.e. (infinity)
    * Rate Limits Setting
      * Check
        * Apply rate limit to muTP protocol
        * Apply rate limit to peers on LAN
  * BitTorrent
    * Privacy
      * Check
        * Enable DHT (decentralized network) to find more peers
        * Enable peer exchange (PeX) to find more peers
        * Encryption Mode
          * Select
            * Require encryption
        * Enable anomymous mode
    * Maximum Active Checking Torrents
      * Select
        * i.e. 1
        * **Note** If you set this value higher than your connection speed may vary
    * Torrent Queueing
      * Maximum Active Downloads
        * Choose number in queue
          * i.e. (Infinity)
          * **Note** Your connection speed may vary
      * Maximum Active Uploads
        * Choose number in queue
          * i.e. 1
          * **Note** If you set this value higher than your connection speed may vary
      * Maximum Active Torrents
        * Choose number in queue
          * i.e. (Infinity)
          * **Note** Your connection speed may vary
      * Check
        * Do not count slow torrents in these limits
      * Download rate threshold
        * Choose number
          * i.e. 10 KiB/s
          * **Note** Your connection speed may vary
      * Upload rate threshold
        * Choose number
          * i.e. 10 KiB/s
          * **Note** If you set this value higher than your connection speed may vary
      * Torrent inactivity timer
        * Choose number
          * i.e. 60 sec
          * **Note** Your connection speed may vary
    * Seeding Limits
      * Check
        * When ratio reaches
          * Choose
            * i.e. 1.00
            * Then choose
              * Pause torrent
  * RSS
    * Leave as default
  * Web UI
    * **NOTE** Only configure if you are going to use the UI
    * Check
      * Web user interface (Remote control)
        * IP Address
          * Input asterisk (*)
          * Choose
            * Port number
              * **NOTE** The port number will be provided or you can choose one
          * Authentication
            * Username
              * i.e. username if any
            * Password
              * i.e. password if any
            * Check
              * Bypass authentication for clients on localhost
          * Security
            * Check
              * Enable clickjacking protection
              * Enable cross-site request forgery (CSRF) proection
              * Enable host header validation
                * Server Domain
                  * Input asterisk (*) 
  * Advanced
    * Confirm Torrent Recheck
      * Check
    * Refresh Interval
      * i.e. 1500 ms
    * Asynchronous I/O threads
      * i.e. 10
    * File Pool Size
      * i.e. 40
    * Outstanding memory when checking torrents
      * i.e. 32 MiB
    * Disk Cache **NOTE** Not on new version
      * i.e. -1 (auto)
    * Dish Cache Expiry Interval **NOTE** Not on new version
      * i.e. 60 s
    * Disk Queue Size
      * i.e. 1024 KiB
    * Outgoing Connections Per Second
      * i.e. 30
    * Socket Backlog Size
      * i.e. 30
    * Outgoing Ports (min) [0: disable]
      * i.e. 0
    * Outgoing Ports (max) [0: disable]
      * i.e. 0
    * Type of Service (ToS) for connections to peers
      * i.e. 4
    * muTP-TCP mix mode algorithnm
      * Choose
        * i.e. Prefer TCP
    * Validate HTTPS tracker certificates
      * Check
    * Server-side requests forgery (SSRF) mitigation
      * Check
    * Upload Slot Behavior
      * Choose
        * i.e. Fixed slots
    * Upload Choking Algorthim
      * Choose
        * i.e. Fastest upload
    * Always announce to all tiers
      * Check
    * Max concurrent http announces
      * i.e. 50
    * Stop tracker timeout
      * i.e. 5 s
    * Peer Turnover diconnect percentage
      * i.e. 4 %
    * Peer Turnover threshold percentage
      * i.e. 90 %
    * Peer Turnover diconnect interval
      * i.e. 300 s
    * Maximum outstanding request to a single peer
      * i.e. 500
