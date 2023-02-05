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
  * `sudo apt install sonarr`
* A common issue experienced by users after installing is related to SSL Certificate Validation issues. This can be resolved by syncing mono's certs
  * `sudo cert-sync /etc/ssl/certs/ca-certificates.crt`
* Add Port 8989 in Router
* Add Port in [UFW](https://github.com/Cuates/ubuntuinstall/tree/main/system/ufw)
  * `sudo ufw allow 8989/tcp`
* Visit Prowlarr webUI
  * http://<ip_address>:8989
  * http://localhost:8989
