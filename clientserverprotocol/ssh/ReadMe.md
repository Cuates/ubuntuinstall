[How to Enable SSH Service in Ubuntu 22.04 LTS](https://ubuntuhandbook.org/index.php/2022/04/enable-ssh-ubuntu-22-04)<br />

* `sudo apt-get install -y ssh`
* `sudo systemctl status sshd`
* `sudo systemctl start sshd`
* `sudo systemctl enable sshd`
* `ifconfig` (Get IP address of the Linux machine)
* Log into the router and port forward SSH (port 22) to the Linux machine, so traffic can be redirected
* `sudo vim /etc/ssh/sshd_config`
  * WAS
    * #Port 22
    * PermitRootLogin=yes
    * OR
    * #PermitRootLogin prohibit-password
  * IS
    * Port 22
    * PermitRootLogin=no
    * OR
    * PermitRootLogin no
* `sudo systemctl restart sshd`
* `sudo systemctl status sshd`
