[How To Install PHP 8.2 on Ubuntu 22.04 20.04 18.04](https://computingforgeeks.com/how-to-install-php-8-2-on-ubuntu/)<br />
[PHP 8.2 Released, How to Install in Ubuntu 22.04 20.04 via PPA](https://ubuntuhandbook.org/index.php/2022/12/php-8-2-ubuntu-ppa/)<br />
[Install PHP 8 for Apache and NGINX on Ubuntu](https://www.linode.com/docs/guides/install-php-8-for-apache-and-nginx-on-ubuntu/)<br />
[How to Install PHP 8 on Ubuntu 20.04 with Apache and Nginx](https://linuxbuz.com/linuxhowto/install-php-8-on-ubuntu-20-04-with-apache-and-nginx)<br />
[php fpm sock failed 13 permission denied Fix it now](https://bobcares.com/blog/php-fpm-sock-failed-13-permission-denied)<br />
[Connection issue with nginx and php fpm on ubuntu 22.04 LTS](https://serverfault.com/questions/1107574/connection-issue-with-nginx-and-php-fpm-on-ubuntu-22-04-lts)<br />
[Configure PHP FPM with Nginx on Ubuntu 22 04](https://www.rosehosting.com/blog/configure-php-fpm-with-nginx-on-ubuntu-22-04)<br />
[How To Install Linux Nginx MySQL PHP LEMP stack on Ubuntu 22 04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-22-04)<br />
[How To 2 Methods To Change TimeZone in Linux](https://www.thegeekstuff.com/2010/09/change-timezone-in-linux/)<br />

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
  * **IMPORTANT STEP**
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
* PHP-FPM Restart Service
  *  `sudo systemctl restart php8.2-fpm`
* PHP-FPM Status Service
  *  `sudo systemctl status php8.2-fpm`
* Create file (if not already created) /etc/nginx/conf.d/<server.name.net>
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
* Modify file /etc/php/8.2/fpm/php.ini (Better performance)
  * `sudo vim /etc/php/8.2/fpm/php.ini`
    * Was
      * <pre>
          upload_max_filesize = 2M
          post_max_size = 8M
          memory_limit = 128M
          max_execution_time = 30
          max_input_vars = 1000
          max_input_time = 60
          date.timezone =
        </pre>
    * Is
      * <pre>
          upload_max_filesize = 32M
          post_max_size = 32M
          memory_limit = 256M
          max_execution_time = 500
          max_input_vars = 3000
          max_input_time = 1000
          date.timezone = America/Los_Angeles
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
  * Visit php file on browser
