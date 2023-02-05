[Sonarr Installation](https://wiki.servarr.com/sonarr/installation)<br />
[Media Info DEB Packages](https://mediaarea.net/repo/deb/)<br />
[Fixing Key is stored in legacy trusted gpg keyring Issue in Ubuntu](https://itsfoss.com/key-is-stored-in-legacy-trusted-gpg/)<br />
[Apt error repository doesnt support architecture i386](https://askubuntu.com/questions/1345751/apt-error-repository-doesnt-support-architecture-i386)<br />

* Add Mono Repo (20.04)
  * `sudo apt install gnupg ca-certificates`
  * `sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF`
  * `echo "deb https://download.mono-project.com/repo/ubuntu stable-focal main" | sudo tee /etc/apt/sources.list.d/mono-official-stable.list`
* Get MediaInfo
  * Visit Media Info DEB Packages to grab the latest DEB version
  * `wget https://mediaarea.net/repo/deb/repo-mediaarea_1.0-21_all.deb && sudo dpkg -i repo-mediaarea_1.0-21_all.deb`
* Add the Sonarr Repo
  * `sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 2009837CBFFD68F45BC180471F4F90DE2A9B4BF8`
  * `echo "deb https://apt.sonarr.tv/ubuntu focal main" | sudo tee /etc/apt/sources.list.d/sonarr.list`
* Apt Update
  * `sudo apt update`
* Install Sonarr
  * `sudo apt install -y sonarr`
  * Package Configuration (window pops open)
    * Specify the user that is used to run Sonarr. The user will be created if it does not already exist. The default 'Sonarr' should work fine for most users. You can specify the user group next.
      * Sonnar user:
        * sonarr
      * Down arrow to Ok and press enter
    * Specify the group that is used to run Sonarr. The group will be craeted if it does not already exist. If the user doesn't already exists then this group will be used as the user's primary group. Any media files created by Sonarr will be writable by this group. It's advisable to keep the group the same between download client, Sonarr and media centers.
      * Sonarr group: (defualt sonarr)
        * media
      * Down arrow to Ok and press enter
    * **WAIT FOR THIS TO FINISH**

* Issues
  * A common issue experienced by users after installing is related to SSL Certificate Validation issues. This can be resolved by syncing mono's certs
    * `sudo cert-sync /etc/ssl/certs/ca-certificates.crt`
  * Check apt-key list
    * `sudo apt-key list`
  * Warning (Make sure to get the proper key for the warning message)
    * <pre>
        W: https://download.mono-project.com/repo/ubuntu/dists/stable-focal/InRelease: Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg), see the DEPRECATION section in apt-key(8) for details.
      </pre>
      * Fix
        * `sudo apt-key export D3D831EF | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/xpjrepo.gpg`
    * <pre>
      W: https://apt.sonarr.tv/ubuntu/dists/focal/InRelease: Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg), see the DEPRECATION section in apt-key(8) for details.
    </pre>
      * Fix
        * `sudo apt-key export 2A9B4BF8 | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/sonarr.gpg`
  * Error
    * <pre>
        N: Skipping acquire of configured file 'main/binary-i386/Packages' as repository 'https://download.mono-project.com/repo/ubuntu stable-focal InRelease' doesn't support architecture 'i386'
      </pre>
      * Fix
        * Modify file /etc/apt/sources.list.d/mono-official-stable.list
          * `sudo vim /etc/apt/sources.list.d/mono-official-stable.list`
            * WAS
              * <pre>
                  deb https://download.mono-project.com/repo/ubuntu stable-focal main
                </pre>
            * IS
              * <pre>
                  deb [arch=amd64] https://download.mono-project.com/repo/ubuntu stable-focal main
                </pre>
          * Save and Exit
  * Update system repos after and warning/error messages are resolved
    * `sudo apt update`

* Add Port 8989 in Router
* Add Port in [UFW](https://github.com/Cuates/ubuntuinstall/tree/main/system/ufw)
  * `sudo ufw allow 8989/tcp`
* Visit Sonarr webUI
  * http://<ip_address>:8989
  * http://localhost:8989
