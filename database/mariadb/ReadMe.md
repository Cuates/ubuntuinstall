[How To Install MariaDB on Ubuntu 22 04](https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-ubuntu-22-04)<br />
[Download MariaDB Server](https://mariadb.org/download)<br />
[Apt error repository doesnt support architecture i386](https://askubuntu.com/questions/1345751/apt-error-repository-doesnt-support-architecture-i386)<br />


* Install dependencies
  * `sudo apt-get install -y apt-transport-https curl`
* Import signing key
  * `sudo curl -o /etc/apt/trusted.gpg.d/mariadb_release_signing_key.asc 'https://mariadb.org/mariadb_release_signing_key.asc'`
* Add repository to source list
  * `sudo sh -c "echo 'deb https://mirrors.xtom.com/mariadb/repo/10.10/ubuntu jammy main' >>/etc/apt/sources.list`

* Error
    * <pre>
        N: Skipping acquire of configured file 'main/binary-i386/Packages' as repository 'https://mirrors.xtom.com/mariadb/repo/10.10/ubuntu jammy InRelease' doesn't support architecture 'i386'
      </pre>
      * Fix
        * Modify file /etc/apt/sources.list
          * `sudo vim /etc/apt/sources.list`
            * WAS
              * <pre>
                  deb https://mirrors.xtom.com/mariadb/repo/10.10/ubuntu jammy main
                </pre>
            * IS
              * <pre>
                  deb [arch=amd64] https://mirrors.xtom.com/mariadb/repo/10.10/ubuntu jammy main
                </pre>
          * Save and Exit
  * Update system repos after and warning/error messages are resolved
    * `sudo apt-get update`
* Install Mariadb
  * `sudo apt-get install -y mariadb-server`
* Secure installation for Mariadb
  * `sudo mariadb-secure-installation`
  * Enter current password for root (enter for none): Enter
  * Switch to unix_socket authentication: Y Enter
  * Change the root password? Y Enter
    * New password: PASSWORD_HERE
    * Re-enter new password:
  * Remove anonymous users: Y Enter
  * Disallow root login remotely: Y Enter
  * Remove test database and access to it: Y Enter
  * Reload privilege tables now: Y Enter
* `mysql -u root -p`
  * exit;
* Service start
  * `sudo systemctl start mariadb`
* Enable at Boot
  * `sudo systemctl enable mariadb`
* Service status
  * `sudo systemctl status mariadb`
* Add Port in [UFW](https://github.com/Cuates/ubuntuinstall/tree/main/system/ufw)
  * `sudo ufw allow 3306/tcp`

* Log into the router and port forward mysql (port 3306) to the Linux machine, so traffic can be redirected (this is optional)
* `sudo mysql`
  * `select user,host,password from mysql.user;`
  * `show databases;`
  * `GRANT ALL ON *.* TO user_name@ip_address IDENTIFIED BY 'user_password' WITH GRANT OPTION;`
    * IP_Address means user can login from a specific IP address
    * "WITH GRANT OPTION" is optional
  * `FLUSH PRIVILEGES;`
    * The above command is to apply changes 
  * `GRANT ALL ON *.* TO user_name@'%' IDENTIFIED BY 'user_password' WITH GRANT OPTION;`
    * '%' means user can login from any IP
    * "WITH GRANT OPTION" is optional
  * `FLUSH PRIVILEGES;`
    * The above command is to apply changes 
  * `GRANT ALL ON <database_name>.* TO user_name@ip_address IDENTIFIED BY 'user_password' WITH GRANT OPTION;`
    * database_name means only to a certain database and no other on the system
    * "WITH GRANT OPTION" is optional
  * Drop User
    * `DROP USER user_name@'ip_address';`
  * `exit;`
* `mariadb --version`

**IMPORTANT NOTE**
* Need to grant permissions within the database to a specific user and IP for external database access

**Setup Database For utf8mb4**
* Modify client.cnf File
  * `sudo vim /etc/my.cnf.d/client.cnf`
    * WAS
      * <pre>
        [client]
        [client-mariadb]
        </pre>

    * IS
      * <pre>
        [client]
        default-character-set = utf8mb4
        [client-mariadb]
        default-character-set = utf8mb4
        </pre>
    * Save and Exit

  * `sudo vim /etc/my.cnf.d/mysql-clients.cnf`
    * WAS
      * <pre>
        [mysql]
        
        [mysql_upgrade]
        
        [mysqladmin]
        
        [mysqlbinlog]
        
        [mysqlcheck]
        
        [mysqldump]
        
        [mysqlimport]
        
        [mysqlshow]
        
        [mysqlslap]
        </pre>
    * IS
      * <pre>
        [mysql]
        default-character-set = utf8mb4
        
        [mysql_upgrade]
        
        [mysqladmin]
        
        [mysqlbinlog]
        
        [mysqlcheck]
        
        [mysqldump]
        
        [mysqlimport]
        
        [mysqlshow]
        
        [mysqlslap]
        </pre>
    * Save and Exit

* Modify server.cnf File
  * `sudo vim /etc/my.cnf.d/server.cnf`
    * WAS
      * <pre>
        [server]
        
        [mysqld]
        
        [galera]
        
        [embedded]
        
        [mariadb]
        
        [mariadb-10.5]
        </pre>
    * IS
      * <pre>
        [server]
        
        [mysqld]
        character-set-client-handshake = FALSE
        character-set-server = utf8mb4
        collation-server = utf8mb4_unicode_520_ci
        init-connect = 'SET NAMES utf8mb4 COLLATE utf8mb4_unicode_520_ci'
        init-connect = 'SET collation_connection = utf8mb4_unicode_520_ci'
        
        [galera]
        
        [embedded]
        
        [mariadb]
        
        [mariadb-10.5]
        </pre>
    * Save and Exit

* Restart MariaDB Services
  * `sudo systemctl restart mariadb`

* Repair And Optimize
  * `mysqlcheck -u root -p --auto-repair --optimize --all-databases`

* Check Database Took Hold Of New Modifications
  * `sudo mysql`
  * `select schema_name, default_character_set_name, default_collation_name from information_schema.schemata where schema_name not in ('mysql', 'information_schema', 'performance_schema', 'sys');`
  * `show session variables where Variable_name like 'character_set\_%' or Variable_name like 'collation%';`
  * `show global variables where Variable_name like 'character_set\_%' or Variable_name like 'collation%';`
  * `show global variables like 'init_connect';`
  * `show variables like '%_server';`
  * `show variables like 'char%';`
  * `show variables like 'collation%';`
  * See which procedures have not been updated to the server’s new character_set_client, collation_connection and Database Collation values
    * `show procedure status;`

**USAGE**<br />
[How To Create A Table In MySQL And MariaDB On An Ubuntu Cloud Server](https://www.digitalocean.com/community/tutorials/how-to-create-a-table-in-mysql-and-mariadb-on-an-ubuntu-cloud-server)<br />
[There Can Be Only One Auto Column](https://stackoverflow.com/questions/8645889/there-can-be-only-one-auto-column)<br />
[Unique](https://www.techonthenet.com/mariadb/unique.php)<br />
[Getting Started With Indexes](https://mariadb.com/kb/en/getting-started-with-indexes/)<br />
[Show Create Table](https://mariadb.com/kb/en/show-create-table/)<br />
[Show Index](https://mariadb.com/kb/en/show-index/)<br />
[Create Index](https://mariadb.com/kb/en/create-index/)<br />
[How To Create Index In MySQL](https://www.javatpoint.com/how-to-create-index-in-mysql)<br />
[Charset Charsets](https://dev.mysql.com/doc/refman/5.7/en/charset-charsets.html)<br />
[How To Backup And Restore MySQL Databases Using The Mysqldump Command](https://www.sqlshack.com/how-to-backup-and-restore-mysql-databases-using-the-mysqldump-command/)<br />
[Use Script Files MySQL](https://www.qualitestgroup.com/resources/knowledge-center/how-to-guide/use-script-files-mysql/)<br />
[Types In MySQL bigint20 VS int20](https://stackoverflow.com/questions/3135804/types-in-mysql-bigint20-vs-int20)<br />
[MariaDB And MySQL Character Set Conversion](https://www.fromdual.com/mariadb-and-mysql-character-set-conversion)<br />
[Truncate Table](https://mariadb.com/kb/en/truncate-table/)<br />
[Listing Stored Procedures In MySQL Database](https://www.mysqltutorial.org/listing-stored-procedures-in-mysql-database.aspx)<br />
[Oist Of Stored Procedures Functions MySQL Command Line](https://stackoverflow.com/questions/733349/list-of-stored-procedures-functions-mysql-command-line)

* Databases
  * `show databases;`

* Database Create
  * `create database if not exists <databasename> default character set utf8mb4 collate utf8mb4_unicode_520_ci;`

* Database Creation
  * `show create database <databasename>;`

* Database Drop
  * `drop database if exists <databasename>;`

* Database Connect
  * `use <databasename>;`

* Tables
  * `show tables;`

* Table Create
  * `create table if not exists <tablename>(`<br />
    `` `tableID` bigint(20) unsigned not null auto_increment, ``<br />
    `` `columnOne` int(11) not null, ``<br />
    `` `columnTwo` varchar(255) collate utf8mb4_unicode_520_ci not null, ``<br />
    `` `columnThree` text collate utf8mb4_unicode_520_ci default null, ``<br />
    `` `columnFour` bit(1) not null default b'0', ``<br />
    `` `columnFive` datetime not null default current_timestamp(), ``<br />
    `` `columnSix` datetime default current_timestamp(), ``<br />
    `` primary key (`tableID`), ``<br />
    `` unique key `UQ_<tablename>_columnOne` (`columnOne`), ``<br />
    `` index `IX_<tablename>_columnTwo` (`columnTwo`) ``<br />
    `) engine=InnoDB default charset=utf8mb4 collate utf8mb4_unicode_520_ci;`<br />

* Table Creation
  * `show create table <tablename>;`

* Table Columns
  * `show columns in <tablename>;`

* Table Indexes
  * `show index from <tablename>;`

* Table Drop
  * `drop table if exists <tablename>;`
  
* Table Select
  * `select tableID, columnOne, columnTwo, columnThree, columnFour, columnFive, columnSix from <tablename>;`

* Table Insert
  * `insert into <tablename> (columnOne, columnTwo, columnThree, columnFour, columnFive, columnSix) values (0, 'columnTwo', 'columnThree', 1, current_timestamp(), current_timestamp());`
  
* Table Update
  * `update <tablename> set columnFour = 1 where columnFour = 0;`
  
* Table Delete
  * `delete from <tablename> where tableID = 1;`

* Table Truncate
  * `truncate table <tablename>;`

* Procedures
  * `show procedure status;`

* Functions
  * `show function status;`

* Restore from IBD files
  * Make sure you have the .ibd files copied to the MariaDB machine
  * Log into the MariaDB database server
  * Login to mysql
    * `sudo mysql`
  * Create Database Instance
    * ````create database if not exists `<database_instance>` default character set utf8mb4 collate utf8mb4_unicode_520_ci;````
  * Change databases
    * `use <database_instance>;`
  * Create the table schema
    * **This step can be ignored if table schema is already obtained**
    * This can be retrieved from the .frm file but you will need a special tool to obtain it
  * Alter table to discard with table space
    * `alter table table_name discard tablespace;`
  * Copy ibd file in question to /var/lib/mysql/path
    * `sudo cp /path/to/ibd /var/lib/mysql/<database_instance>;`
  * Make sure all .ibd files are chown to mysql:mysql user
    * `sudo chown mysql:mysql *.ibd`
  * Alter table with the ibd data by using import
    * `alter table table_name import tablespace;`

* Export
  * Open a terminal of choice to execute the following dump command
  * The following command will dump the content of the database to a file named with current date-time stamp
  * NOTE: You will want to take a backup of the database with the mariadb 'root' user name, so you will need the root mariadb password to continue the SQL dump command
  * You will be prompted to input the mariadb user name's password you entered into the dump command
    * Dump for a certain database;
      * NOTE: You will have to create your Database to import the dump back into the database as the command dumps everything inside the database to a file
        * ````mysqldump -u <mariadb_user_name> -p <database_instance> -R -E --triggers --single-transaction > database_instance_`date +%d_%b_%Y_%H_%M_%S`.sql````
    * Dump all databases
      * ````mysqldump -u <mariadb_user_name> -p -A -R -E --triggers --single-transaction > database_instance_`date +%d_%b_%Y_%H_%M_%S`.sql````
    * Flag meaning
      * -A For all databases &#40;you can also use --all-databases&#41;
      * -R For all routines &#40;stored procedures & triggers&#41;
      * -E For all events
      * --single-transaction Without locking the tables i.e., without interrupting any connection &#40;R/W&#41;
* Check Export If It's A Legitimate SQL Dump File
  * `head -n 5 database_instance_dump.sql`

* Import
  * Make sure your database is all setup and ready for database and table creation
  * Make sure you have the dump from your old database system
  * First create the database
    * NOTE: The following command can be inserted at the top of the dump file, you will be able to execute everything at once instead of multiple executions
      * ````create database if not exists `<database_instance>` default character set utf8mb4 collate utf8mb4_unicode_520_ci;````
  * Second create users that will access the database
  * Third open terminal
    * `mysql -u username -p new_database < data-dump.sql`
      * username is the username you can log in to the database with
      * newdatabase is the name of the freshly created database
      * data-dump.sql is the data dump file to be imported, located in the current directory
      * NOTE: make sure to look at your user table as this may have updated with the contents of the dump
  * Third open the dump in a mariadb client (GUI version using dump file)
    * The dump will consist of various select, insert, delete, and so on statements
    * Make sure to include the following command at the top of the dump; this will make sure you are using the correct database
    * NOTE: If the create database is inserted at the top of the dump file then this will go below the create database command
      * `use <database_instance>;`
      * Execute the entire dump and wait for the commands to finish
  * You should now be back up and running with your old database system on your new system
