[Certbot](https://certbot.eff.org)<br />
[Enable HTTP 2 In Nginx](https://www.tecmint.com/enable-http-2-in-nginx/)<br />

* Prerequisite
  * Nginx
    * [Nginx Web Server](https://github.com/Cuates/centosinstall/tree/master/webserver/nginx)
  * NoIP
    * [Domain Name](https://github.com/Cuates/lampcentosinstall/tree/master/system/domainname)
  * Snap
    * [Installing snap on Ubuntu](https://snapcraft.io/docs/installing-snap-on-ubuntu)

* Set Nginx virtual host before proceeding with Certbot
  * Only perform the following commands if the <domain_name>.conf file does not exist
  * `sudo vim /etc/nginx/conf.d/<domain_name>`
  * Append the following code:
    * <pre>
      # http port 80
      server {
        listen      80;
        server_name &lt;domain_name&gt;;
        access_log  /var/log/nginx/http_&lt;domain_name&gt;_access.log;
        error_log   /var/log/nginx/http_&lt;domain_name&gt;_error.log;
        root        /usr/share/nginx/html;
      }
      </pre>
  * Save and exit
  * Test Nginx
    * `sudo nginx -t`
  * Restart Nginx Service
    * `sudo systemctl restart nginx`
* Install SSH onto the server
* Install snapd
  * `sudo apt-get install -y snapd`
  * Snapd Status
    * `sudo systemctl status snapd.socket`
* Install Certbot
  * `sudo snap install --classic certbot`
    * WAIT FOR THIS TO FINISH
* Prepare the Certbot command
  * `sudo ln -s /snap/bin/certbot /usr/bin/certbot`
* Choose how you'd like to run Certbot Either get and install your certificates
  * Run this command to get a certificate and have Certbot edit your Nginx configuration automatically to serve it, turning on HTTPS access in a single step.
    * `sudo certbot --nginx`
      * Enter your email address when prompted for urgent renewal and security notices
      * Enter Y or N to agree to the terms and conditions when prompted: Y
      * Choose Y or N when prompted to share your email address with the EFF: N
      * Choose 1 when prompted about which domain you want the certificate for (there should only be one)
        * If none were found, then proceed to add your domain name
          * No names were found in your configuration files. Please enter in your domain name(s) (comma and/or space separated)  (Enter 'c' to cancel): yourdomain
          * Created an SSL vhost at sudo vim /etc/nginx/conf.d/<yourdomainname>-le-ssl.conf
            Deploying Certificate to VirtualHost sudo vim /etc/nginx/conf.d/<yourdomainname>-le-ssl.conf
            Redirecting vhost in sudo vim /etc/nginx/conf.d/<yourdomainname>.conf to ssl vhost in sudo vim /etc/nginx/conf.d/<yourdomainname>-le-ssl.conf
          * Create a virutalhost first
          * Re-execute the command above
    * IMPORTANT NOTES:
      - Congratulations! Your certificate and chain have been saved at:
        /etc/letsencrypt/live/\<yourdomainname\>/fullchain.pem
        Your key file has been saved at:
        /etc/letsencrypt/live/\<yourdomainname\>/privkey.pem
        Your cert will expire on 2021-01-12. To obtain a new or tweaked
        version of this certificate in the future, simply run certbot again
        with the "certonly" option. To non-interactively renew *all* of
        your certificates, run "certbot renew". You should make a
        secure backup of this folder now. This configuration directory will
        also contain certificates and private keys obtained by Certbot so
        making regular backups of this folder is ideal.

        Saving debug log to /var/log/letsencrypt/letsencrypt.log
        Plugins selected: Authenticator nginx, Installer nginx

        Which names would you like to activate HTTPS for?
        - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
        1: <domain_name>
        - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
        Select the appropriate numbers separated by commas and/or spaces, or leave input
        blank to select all options shown (Enter 'c' to cancel): 1
        Cert not yet due for renewal

        You have an existing certificate that has exactly the same domains or certificate name you requested and isn't close to expiry.
        (ref: /etc/letsencrypt/renewal/<domain_name>.conf)

        What would you like to do?
        - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
        1: Attempt to reinstall this existing certificate
        2: Renew & replace the cert (may be subject to CA rate limits)
        - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
        Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 1
        Keeping the existing certificate
        Deploying Certificate to VirtualHost /etc/nginx/conf.d/<domain_name>.conf
        Redirecting all traffic on port 80 to ssl in /etc/nginx/conf.d/<domain_name>.conf

        - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
        Congratulations! You have successfully enabled https://<domain_name>
        - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

        IMPORTANT NOTES:
        - Congratulations! Your certificate and chain have been saved at:
          /etc/letsencrypt/live/<domain_name>/fullchain.pem
          Your key file has been saved at:
          /etc/letsencrypt/live/<domain_name>/privkey.pem
          Your cert will expire on 2021-01-12. To obtain a new or tweaked
          version of this certificate in the future, simply run certbot again
          with the "certonly" option. To non-interactively renew *all* of
          your certificates, run "certbot renew"
        - If you like Certbot, please consider supporting our work by:

          Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
          Donating to EFF:                    https://eff.org/donate-le
  * Or, just get a certificate
    * `sudo certbot certonly --nginx`
* Test automatic renewal
  * `sudo certbot renew --dry-run`
  * The command to renew certbot is installed in one of the following locations:
    /etc/crontab/
    /etc/cron.*/*
    systemctl list-timers
* Add Protocols h2 (HTTP/2) to the port 443 Certbot generated file
  * `sudo vim /etc/nginx/conf.d/<domain_name>-le-ssl.conf`
    * Paste the HTTP/2 section into the below file
      * <pre>
        server {
          server_name &lt;yourdomainname&gt; www.&lt;yourdomainname&gt;;
          access_log  /var/log/nginx/&lt;yourdomainname&gt;_access.log;
          error_log  /var/log/nginx/&lt;yourdomainname&gt;_error.log;

          listen 443 ssl http2; # managed by Certbot

          ssl_certificate /etc/letsencrypt/live/&lt;yourdomainname&gt;/fullchain.pem; # managed by Certbot
          ssl_certificate_key /etc/letsencrypt/live/&lt;yourdomainname&gt;/privkey.pem; # managed by Certbot
          include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
          ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
        }
        </pre>
  * Save and exit
  * Test Nginx
    * `sudo nginx -t`
  * Restart Nginx Service
    * `sudo systemctl restart nginx`
  * Test HTTP/2 Protocol
    * `curl -I --http2 -s https://<domain_name>/ | grep HTTP`
      * A message of HTTP/2 200 will display
      * Else a message of HTTP/1.1 200 OK if HTTP/2 did not work
* Visit your router setting for port forwarding
  * Set the IP address to the port 443 in the port forwarding section of your router
* Confirm that Certbot worked
  * Navigate to your site using the https prefix
    * e.g. https://yourdomainname
