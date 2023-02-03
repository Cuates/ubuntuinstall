[How To Install PHP 8.2 on Ubuntu 22.04 20.04 18.04](https://computingforgeeks.com/how-to-install-php-8-2-on-ubuntu/)<br />
[PHP 8.2 Released, How to Install in Ubuntu 22.04 20.04 via PPA](https://ubuntuhandbook.org/index.php/2022/12/php-8-2-ubuntu-ppa/)<br />
[Install PHP 8 for Apache and NGINX on Ubuntu](https://www.linode.com/docs/guides/install-php-8-for-apache-and-nginx-on-ubuntu/)<br />

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
* Check PHP-FPM Service is running
  *  `sudo systemctl status php8.2-fpm`
* Configure sites-available Default file
  * `sudo vim /etc/nginx/sites-available/default`
    * Add index.php to the other index lines
      * index index.php ...
    * Uncomment the following lines
      * <pre>
          location ~ \.php$ {
            include snippets/fastcgi-php.conf;
            fastcgi_pass unix:/run/php/php8.2-fpm.sock;
          }
        </pre>
  * Save and exit
* Verify Nginx Configuration
  * `sudo nginx -t`
* Restart Nginx Service
  * `sudo systemctl restart nginx`
