* Prerequisite
  * [Nginx](https://github.com/Cuates/ubuntuinstall/tree/main/webserver/nginx)

* How to set up an external DNS for your server
  * Create an account with a free DNS of your choice
    * [NoIP](https://www.noip.com/)
      * If asked to do so, create a DNS to the ISP you are trying to make public
  * Once the account has been setup, go to your router for modifications
    * Login to your router
      * Navigate to the port forwarding section
        * Add the internal IP address of the server you are trying to make public with the port the server is listening on
