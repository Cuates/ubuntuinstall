[How To Install Nginx on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04)<br />
[How to Install Nginx Mainline on Ubuntu 22.04 20.04](https://www.linuxcapable.com/install-nginx-mainline-on-ubuntu-linux/)<br />

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
* Stop Service
  * `sudo systemctl stop nginx`
* Remove Nginx
  * `sudo apt autoremove -y nginx*`
