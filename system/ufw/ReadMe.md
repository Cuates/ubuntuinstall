[How To Set Up a Firewall with UFW on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-20-04)<br />
[UFW](https://help.ubuntu.com/community/UFW)<br />

* List services
  * `less /etc/services`
* Display rules in the firewall
  * `sudo ufw status verbose`
* Allow known services by name
  * `sudo ufw allow <service_name>`
  * i.e. `sudo ufw allow ssh`
* Allow known services by port
  * `sudo ufw allow <port_number>`
  * i.e. `sudo ufw allow 2222/tcp`
* Deny Existing Rule
  * `sudo ufw deny <port_number>`
  * i.e. `sudo ufw deny 2222`
* Delete Rule by Number
  * `sudo ufw status numbered`
  * `sudo ufw delete <status_number>`
    * i.e. `sudo ufw delete 1`
    * Proceed with operation (y|n)? y
