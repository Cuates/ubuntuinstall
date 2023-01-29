[How To Set Up a Firewall with UFW on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-20-04)<br />
[UFW](https://help.ubuntu.com/community/UFW)<br />

* List services
  * `less /etc/services`
* Allow known services by name
  * `sudo ufw allow <service_name>`
  * i.e. `sudo ufw allow ssh`
* Allow known services by port
  * `sudo ufw allow <port_number>`
  * i.e. `sudo ufw allow 22`
* Deny Existing Rule
  * `sudo ufw deny <port_number>`
  * i.e. `sudo ufw deny 22`
* Delete Deny Existing Rule
  * `sudo ufw delete deny <port_number>`
  * i.e. `sudo ufw delete deny 1234`
