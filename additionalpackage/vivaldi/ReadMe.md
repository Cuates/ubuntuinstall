[How to install Vivaldi Browser on Ubuntu 22.04](https://www.tutsmake.com/how-to-install-vivaldi-browser-on-ubuntu-22.04/)<br />
[How to Install Vivaldi on Ubuntu 22.04](https://www.linuxcapable.com/install-vivaldi-browser-on-ubuntu-linux/)<br />

* Add GPG Key
  * `sudo apt-get install -y wget gnupg2 ubuntu-keyring`
* Integrate GPG Key
  * `curl -fSsL https://repo.vivaldi.com/archive/linux_signing_key.pub | sudo gpg --dearmor | sudo tee /usr/share/keyrings/vivaldi.gpg > /dev/null`
* Add Vivaldi Repository
  * `echo deb [arch=amd64 signed-by=/usr/share/keyrings/vivaldi.gpg] https://repo.vivaldi.com/archive/deb/ stable main | sudo tee /etc/apt/sources.list.d/vivaldi.list`
* Update System Dependencies
  * `sudo apt-get update`
* Install Vivaldi
  * `sudo apt-get install -y vivaldi-stable`
