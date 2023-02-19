[How to install Docker on Ubuntu 22 04](https://linuxconfig.org/how-to-install-docker-on-ubuntu-22-04)<br />
[Docker Desktop](https://www.docker.com/products/docker-desktop)<br />
[How To Install and Use Docker on Ubuntu 22 04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04)<br />
[How To Install and Use Docker Compose on Ubuntu 22 04](https://digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-22-04)<br />
[Install and Configure Docker Compose on Ubuntu 22 04 LTS Jammy](https://www.how2shout.com/linux/install-and-configure-docker-compose-on-ubuntu-22-04-lts-jammy)<br />
[Komga Windows Install and Update](https://komga.org/installation/thirdparty.html#windows-installer-and-updater)<br />
[Komga Optional COnfiguration](https://komga.org/installation/configuration.html#optional-configuration)<br />
[Komga Baseurl](https://komga.org/installation/configuration.html#server-servlet-context-path-server-servlet-context-path-baseurl)<br />
[Komga Gotson](https://github.com/gotson/komga/issues/353)<br />

* Docker
  * Update System
    * `sudo apt-get update`
  * Install prerequisite packages
    * `sudo apt install apt-transport-https ca-certificates curl software-properties-common`
  * Add GPG Key for the official Docker repo
    * `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`
  * Add Docker repo to APT sources
    * `echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`
  * Update existing list of packages
    * `sudo apt-get update`
  * Check if installing from Docker instead of Ubuntu
    * `apt-cache policy docker-ce`
  * Install Docker
    * `sudo apt-get install docker-ce`
  * Check system status
    * `sudo systemctl status docker`
  * Add username to the docker group
    * `sudo usermod -aG docker <username>`
  * Log out and back in for users and groups to take hold
    * NOTE: If log out does not work, then restart your system
* Docker-Compose
