[update ubuntu command line](https://www.makeuseof.com/update-ubuntu-command-line/)<br />
[Why are upgradable packages not upgraded](https://askubuntu.com/questions/1181852/why-are-upgrable-packages-not-upgraded)<br />

* `cat /etc/*-release`
* `sudo apt-get clean all`
* `sudo apt-get update`
* `sudo apt-get upgrade -y`
* See Upgradable List
* `sudo apt-get list --upgradable`
* If you see not upgraded packages, then perform the following
  * `sudo apt-get install <list_of_packages_kept_back>`
* To remove packages no longer required
* `sudo apt-get autoremove`
  * Do you want to continue? [Y/n] Y
