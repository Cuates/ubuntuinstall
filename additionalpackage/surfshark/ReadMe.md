[How to set up Surfshark on Linux](https://support.surfshark.com/hc/en-us/articles/5067279648146-How-to-set-up-Surfshark-on-Linux-)<br />

* Open terminal and type the follwoing commands
  * Curl needs to be install first
* `curl -f https://downloads.surfshark.com/linux/debian-install.sh --output surfshark-install.sh`
* `cat surfshark-install.sh`
* `sudo sh surfshark-install.sh`
  * Type password with root access
* `sudo apt-get update`
* `sudo apt-get upgrade`

* An error occured during the signature verification. The repository is not updated and the previous index files will be used. GPG error: stretch InRelease the following signatures were invalid Surfshark package maintainer
 * Resolution
   * Execute the steps above
