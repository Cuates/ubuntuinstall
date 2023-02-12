[How to Install Jellyfin Media Server on Ubuntu 22 04 20 04 Server Desktop](https://www.linuxbabe.com/ubuntu/install-jellyfin-media-server-ubuntu-20-04)<br />
[How to Install Jellyfin Media Server on Ubuntu 22 04 LTS](https://www.linuxcapable.com/how-to-install-jellyfin-media-server-on-ubuntu-22-04-lts)<br />
[Jellyfin Skin Manager Plugin](https://github.com/danieladov/jellyfin-plugin-skin-manager)<br />
[Jellyfin Plugin ThePornDB](https://github.com/ThePornDatabase/Jellyfin.Plugin.ThePornDB)<br />
[Intro Skipper](https://github.com/ergoz/jellyfin-intro-skipper)<br />

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
* Add username to group
  * `sudo adduser <username> jellyfin`
* Add Port 8096 in Router
* Add Port in [UFW](https://github.com/Cuates/ubuntuinstall/tree/main/system/ufw)
  * `sudo ufw allow 8096/tcp`

* **Optional** Steps for custom cache and metadata locations
  * Specify a custom location for downloaded artwork and metadata.
    * Make the following changes to the metadata folder. This helps in having more space allocated to the metadata items in the Jellyfin application. Perform the same steps for cache as well.
    * Find a location that has lots of space to store metadata and cache from the media you are going to save.
    * Preferably an external hard drive
      * Perform the following in the command line
        * Change directory to external hard drive
          * `cd /path/to/external/hard/drive/with/lots/of/space/`
        * Create the Jellyfin directory
          * `mkdir jellyfin`
        * Change directory to new Jellyfin folder
          * `cd jellyfin/`
        * Create the cache directory within the new Jellyfin directory path
          * `mkdir cache`
        * Change directory to new cache folder
          * `cd cache/`
        * Make a note of where the new cache directory is located
          * `pwd`
        * Change directory to new Jellyfin folder
          * `cd jellyfin/`
          * Create the metadata directory within the new Jellyfin directory path
            * `mkdir metadata`
          * Change directory to new metadata folder
            * `cd metadata/`
          * Make a note of where the new metadata directory is located
            * `pwd`
          * Change directory to the parent of the new Jellyfin folder that was created
            * `cd /path/to/external/hard/drive/with/lots/of/space/where/the/new/jellyfin/was/created`
          * The folder will need jellyfin permissions
            * `sudo chown -R jellyfin:jellyfin jellyfin/`
          * Jellyfin folder will need special permission to be accessible to the Jellyfin application
            * default permissions for Jellyfin is drwxr-x---
              * Change permission to match the default Jellyfin folder
                * `sudo chmod -R 750 jellyfin`
          * Cache folder will need special permission to be accessible to the Jellyfin application
            * default permissions for Jellyfin's cache is drwxr-x---
              * Change permission to match the default Jellyfin's cache folder
                * `sudo chmod -R 750 cache`
          * Metadata folder will need special permission to be accessible to the Jellyfin application
            * default permissions for Jellyfin's metadata is drwxr-xr-x
              * Change permission to match the default Jellyfin's metadata folder
                * `sudo chmod -R 755 metadata`
    * Navigate to the Jellyfin server to modify locations for both Cache path and Metadata path
      * Go to Dashboard --> General --> Cache Path
        * Click the search icon to the right of the input field
        * Click on the 3 dots below the Folder input field
        * Find the new path the metadata will live in
        * Old path was `/var/cache/jellyfin`
        * New path is /path/to/external/hard/drive/with/lots/of/space/with/the/new/jellyfin/cache
      * Go to Dashboard --> General --> Metadata Path
        * Click the search icon to the right of the input field
        * Click on the 3 dots below the Folder input field
        * Find the new path the metadata will live in
        * Old path was `/var/lib/jellyfin/metadata`
        * New path is /path/to/external/hard/drive/with/lots/of/space/with/the/new/jellyfin/metadata


* Jellyfin Setup
 * Preferred Display Lanuage
   * English
   * Click Next
 * Username and Password
   * Click Next
 * Wait to add media library (After plugins have been installed)
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
 * Plugins After configuration has been setup
   * Important Add Plugins Before creating up libraries
   * Some plugins may already be installed by default
    * AniDB
    * AniList
    * AniSearch
    * AudioDB
    * Bookshelf
    * Fanar
    * Intro Skipper
      * [Manifest JSON](https://raw.githubusercontent.com/ConfusedPolarBear/intro-skipper/master/manifest.json)
    * Kitsu
    * Merge Versions
    * MusicBrainz
    * OMDb
    * Open Subtitles
    * Playback Reporting
    * Reports
    * Simkl
    * SkinManager
      * [Manifest JSON](https://raw.githubusercontent.com/danieladov/JellyfinPluginManifest/master/manifest.json)
    * Studio Images
    * TMDb
    * TVmaze
    * ThePornDB
      * [Manifest JSON](https://raw.githubusercontent.com/ThePornDatabase/Jellyfin.Plugin.ThePornDB/main/manifest.json)
    * TheTVDB
