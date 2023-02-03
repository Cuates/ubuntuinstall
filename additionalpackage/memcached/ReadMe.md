(How to Install Memcached on Ubuntu 22.04 20.04)[https://www.linuxcapable.com/install-memcached-on-ubuntu-linux/]<br />

* Install Memcached
  * `sudo apt install -y memcached libmemcached-tools`
* Memcached Version
  * `memcached --version`
* Check Status
  * `systemctl status memcached`
* Start and Enable memcached
  * `sudo systemctl enabled memcached --now`
* Verify actively listening to localhost on the default port
  * `ps -ef | grep memcached`
* Configure Memcached
  * `sudo vim /etc/memcached.conf`
    * Check the -l parameter is present on the following line
      * -l 127.0.0.1
    * Disable UDF
      * -U 0
    * Change default memory allocation
      * -m 2048
    * Save and exit
  * Restart Memcached
    * `sudo systemctl restart memcached`
* Restart Nginx
  * `sudo systemctl restart nginx`
