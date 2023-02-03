[How To Install PHP 8.2 on Ubuntu 22.04 20.04 18.04](https://computingforgeeks.com/how-to-install-php-8-2-on-ubuntu/)<br />
[PHP 8.2 Released, How to Install in Ubuntu 22.04 20.04 via PPA](https://ubuntuhandbook.org/index.php/2022/12/php-8-2-ubuntu-ppa/)<br />
[Install PHP 8 for Apache and NGINX on Ubuntu](https://www.linode.com/docs/guides/install-php-8-for-apache-and-nginx-on-ubuntu/)<br />
[How to Install PHP 8 on Ubuntu 20.04 with Apache and Nginx](https://linuxbuz.com/linuxhowto/install-php-8-on-ubuntu-20-04-with-apache-and-nginx)<br />
[php fpm sock failed 13 permission denied Fix it now](https://bobcares.com/blog/php-fpm-sock-failed-13-permission-denied)<br />

* Update System
  * `sudo apt-get update`
* Install Dependencies
  * `sudo apt-get install -y lsb-release gnupg2 ca-certificates apt-transport-https software-properties-common`
* Add PPA to the System
  * `sudo LC_ALL=C.UTF-8 sudo add-apt-repository ppa:ondrej/php`
* Update the repository with PPA
  * `sudo apt-get update`
* Install PHP 8.2
  * `sudo apt install -y php8.2`
* PHP Version
  * `php -v`
  * `php --version`
* Install Modules
  * `sudo apt-get install -y php8.2-{cli,common,sybase,curl,fpm,mysql,opcache,gd,xml,mbstring,pgsql,odbc,memcached,bcmatch,dba,imap,intl,ldap,mcrypt,tidy,xmlrpc}`
* Modify file /etc/php/8.2/fpm/pool.d/www.conf
  * `sudo vim /etc/php/8.2/fpm/pool.d/www.conf`
    * WAS
      * <pre>
          user = www-data
          group = www-data
          listen.owner = www-data
          listen.group = www-data
        </pre>
    * IS
      * <pre>
          user = nginx
          group = nginx
          listen.owner = nginx
          listen.group = nginx
        </pre>
  * Save and Exit
* Check PHP-FPM Service is running
  *  `sudo systemctl status php8.2-fpm`
* Create file /etc/nginx/conf.d/<server.name.net>
  * `sudo vim /etct/nginx/conf.d/<server.name.net>`
    * Add the following
    * <pre>
        serve {
          root /var/www/html;
          index index.html index.htm index.php
          
          server_name <server.name.net>;
          
          location / {
            try_files $uri $uri/ =404;
          }
          
          location ~ \.php$ {
            try_files $uri =404;
            fastcgi_intercept_errors on;
            fastcgi_index index.php;
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            fastcgi_slit_path_info ^(.+\.php)(/.+)$;
            fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
          }
        }
      </pre>
  * Save and Exit
* Verify Nginx Configuration
  * `sudo nginx -t`
* Restart Nginx Service
  * `sudo systemctl restart nginx`
* Create a simple PHP file
  * `sudo vim /var/www/html/info.php`
    * <?php phpinfo(); ?>
  * Save and exit
  * Visit php file
* ADD PHP better performance from URL https://linuxbuz.com/linuxhowto/install-php-8-on-ubuntu-20-04-with-apache-and-nginx
* CHANGE TIMEZONE from URL https://www.thegeekstuff.com/2010/09/change-timezone-in-linux/
* https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-22-04
