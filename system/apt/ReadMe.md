[4 Easy Methods To Remove An APT Repository In Ubuntu](https://www.linuxfordevices.com/tutorials/ubuntu/remove-an-apt-repository-ubuntu)<br />
[How to correctly remove an Ubuntu 20.04 repo](https://askubuntu.com/questions/1423696/how-to-correctly-remove-an-ubuntu-20-04-repo)<br />

Package Manager Version
* `sudo apt --version`
Add PPA to apt package manager
* `sudo add-apt-repository <ppa:package_name/ppa>`
* i.e. `sudo add-apt-repository <ppa:deadsnakes/ppa>`
Remove PPA from apt package manager
* `sudo add-apt-repository --remove <ppa:package_name/ppa>`
