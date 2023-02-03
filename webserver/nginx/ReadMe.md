[How To Install Nginx on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04)<br />
[How to Install Nginx Mainline on Ubuntu 22.04 20.04](https://www.linuxcapable.com/install-nginx-mainline-on-ubuntu-linux/)<br />
[How to Install Nginx on Ubuntu 22.04 LTS](https://www.linuxcapable.com/how-to-install-nginx-on-ubuntu-22-04-lts/)<br />

* Set Stable Repository
  * `sudo apt install -y curl gnupg2 ca-certificates lsb-release dirmngr software-properties-common apt-transport-https`
* Download and Add Nginx GPG Key
  * `curl -fSsL https://nginx.org/keys/nginx_signing.key | sudo gpg --dearmor | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null`
* Verify GPG Key
  *  `gpg --dry-run --quiet --import --import-options import-show /usr/share/keyrings/nginx-archive-keyring.gpg`
* Import Nginx Stable Repository
  * `echo "deb [arch=amd64,arm64 signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] http://nginx.org/packages/ubuntu `lsb_release -cs` nginx" | sudo tee /etc/apt/sources.list.d/nginx.list`
* APT pinning to prefer Nginx packages over Default Ubuntu Repositories
  * `echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" | sudo tee /etc/apt/preferences.d/99nginx`
* Update APT Repositories
  * `sudo apt-get update`
* Install Nginx
  * `sudo apt-get install -y nginx`
* Verify Nginx Install
  * `apt-cache policy nginx`
* Start and Enable Nginx
  * `sudo systemctl enable nginx --now`
* List available applications
  * `sudo ufw app list`
  * Nginx allows all of Nginx
    * Nginx HTTP
    * Nginx HTTPS
    * Nginx FULL
* Allows UFW Nginx (This example allowed both HTTP and HTTPS)
  * `sudo ufw allow 'Nginx FULL'`
* Check UFW Status
  * `sudo ufw status`
* Check Nginx is visible via browser
  * [http://localhost](http://localhost)
* Uninstall Nginx
  * Stop Service
    * `sudo systemctl stop nginx`
  * Remove Nginx
    * `sudo apt autoremove -y nginx*`
* Additional Commands
  * Start service and Enable service at boot up
    * `sudo systemctl enable --now nginx`
  * Stop Nginx Service
    * `sudo systemctl stop nginx`
  * Restart Nginx Service
    * `sudo systemctl restart nginx`
  * Reload the configuration files without stopping the service
    * `sudo systemctl reload nginx`
  * To check if it is enabled on startup
    * `sudo systemctl is-enabled nginx`
  * Disable the Nginx service from startup
    * `sudo systemctl disable nginx`
  * To test Nginx
    * `sudo service nginx configtest`
    * `sudo nginx -t`
