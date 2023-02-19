[Quickstart: Install SQL Server and create a database on Ubuntu](https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-ubuntu?view=sql-server-ver16)<br />

**NOTE: DO NOT FOLLOW THESE STEPS AS MSSQL SERVER DOES NOT SUPPORT UBUNTU 22.04**
**NEED TO REVISIT THIS INSTALL AT A LATER TIME**

* Import GPG key
  * `wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo tee /etc/apt/trusted.gpg.d/microsoft.asc`
* Register SQL Server Ubuntu repo
  * `sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/20.04/mssql-server-2022.list)"`
* Update system
  * `sudo apt-get update`
* Install mssql server
  * `sudo apt-get install -y mssql-server`

* Install MS SQL server on RHEL 8 / CentOS 8
  * `sudo dnf makecache`
  * `sudo dnf clean all`
  * `sudo dnf -y install mssql-server`
    * **WAIT FOR THIS TO FINISH**

* Install SQL Server command-line tools
  * `sudo dnf -y install mssql-tools unixODBC-devel`
    * Accept License
      * YES Enter
      * YES Enter
  * `sudo dnf -y update mssql-tools`
  * `sudo systemctl restart mssql-server`

* Confirm installation
  * `rpm -qi mssql-server`
  * `dnf info mssql-server`
  * `rpm -qi mssql-tools`

* Initialize MS SQL Database Engine
  * `sudo /opt/mssql/bin/mssql-conf setup`
    * Select an edition you’d like to use
      * \# Enter
      * **NOTE** DOES NOT ASK FOR LICENSE KEY
    * Accept the license terms
      * Yes Enter
    * Set SQL Server system administrator password
      * Password Enter
      * Confirm Password Enter

* The service should be started and set to start at boot
  * `systemctl status mssql-server.service`
  * `systemctl is-enabled mssql-server.service`

* Add /opt/mssql/bin/ to your $PATH variable
  * `echo 'export PATH=$PATH:/opt/mssql/bin:/opt/mssql-tools/bin' | sudo tee /etc/profile.d/mssql.sh`

* Source the file to start using MS SQL executable binaries in your current shell session
  * `source /etc/profile.d/mssql.sh`

* If you have an active Firewalld service, allow SQL Server ports for remote hosts to connect
  * `sudo firewall-cmd --get-services`
  * `sudo firewall-cmd --zone=public --permanent --add-service=mssql`
  * `sudo firewall-cmd --reload`
  * `sudo firewall-cmd --list-services`
  * `sudo firewall-cmd --info-service mssql`
  * `sudo firewall-cmd --list-all`

* Install Polybase
  * `sudo dnf -y install mssql-server-polybase`
  * `sudo systemctl restart mssql-server`
  * `sudo dnf -y install mssql-server-polybase-hadoop`
  * `sudo systemctl restart mssql-launchpadd`

**USAGE**<br />
* Test SQL Server
  * Connect to the SQL Server and verify it is working
    * `sqlcmd -S localhost -U SA`
      * Authenticate with the password set above
        * Password Enter

* Show Database users
  * `select name from sysusers;`
  * `go`

* Create a test database
  * `create database mytestdb`
  * `select name from sys.databases`
  * `go`
  * `use mytestdb`
  * `create table inventory (id int, name nvarchar(50), quantity int)`
  * `insert into inventory values (1, 'banana', 150);`
  * `go`
  * `insert into inventory values (2, 'orange', 154);`
  * `go`
  * `select top 1 * from inventory;`
  * `go`

* Show databases on the SQL Server
  * `select name, database_id from sys.databases;`
  * `go`

* Drop a database
  * `use master;`
  * `go`
  * `drop database mytestdb;`
  * `go`
  * `select name, database_id from sys.databases;`
  * `go`
  * `exit`

* Create a user for the SQL server via GUI
  * Open Microsoft SQL Server Management Studio
    * Sign in as a root user for the SQL server
      * Expand the SQL server you just connected to
        * Expand Security under the server you just connected to
          * Right click on "Logins"
            * Click on "New Login..."
              * Under the General tab perform the necessary changes
                * Type the "Login name"
                * Make sure the SQL Server authentication radio button is selected
                  * Input password and confirm password
                  * Uncheck
                    * Enforce password policy
                    * Enforce password expiration
                    * User must change password at next login
                * Make any necessary modification where needed
                * Set Default database
                * Set Default language
              * Adjust anything else for any of the other tabs if needed
              * Click button "OK" when you are done with the modifications
  * Sign out of the current user
  * Sign in as the newly created user to make sure everything worked

* Import
  * The following is for a Linux MSSQL server
    * Open a terminal of choice
      * Sign into the linux server as root user
        * Copy the .bak file you created when you exported the database to the following location '/var/opt/mssql/data'
          * `cp ~/media.bak .`
        * Adjust the permissions to the mssql user, this will allow the mssql user to interact with the backup file
          * `chown mssql:mssql media.bak`
  * Open Microsoft SQL Server Management Studio on Windows or any operating system that will open the SSMS application
    * Sign in as a root user for the SQL server
      * Expand the SQL server you just connected to
        * Right click on "Databases"
            * Click on "Restore Files and Filegroups"
              * Under the General tab perform the necessary changes
                * Type the "To database" name
                * Select "From device" radio button
                  * Click on the 3 dots to the right of the radio button
                  * Make sure "Backup media type is "File"
                  * Click button "Add"
                  * Select the "data" folder in the tree structure
                  * Select the backup of choice
                    * Click button "OK"
                  * Click button "OK"
                * Check "Restore" of the backup you created earlier
                  * You see which one to check, look at the "Start Date" and "Finish Date" columns for the latest backup
              * Adjust anything else for any of the other tabs if needed
              * Click button "OK" when you are done with the modifications
              * Wait for the "Executing" to finish
                * This may take some time depending on how much data was backup
              * You will get a pop window stating
                * "Database 'database_name' restored successfully."
                  * Click button "OK"
        * Right click on "Databases"
          * Click "Refresh"
      * You have successfully imported your backup from your old SQL server

* Check new user's permission to make sure they are able to access the newly created database.
  * Open Microsoft SQL Server Management Studio
      * Sign in as a root user for the SQL server
        * Expand the SQL server you just connected to
          * Expand Database under the server you just connected to
            * Expand Security under the database you expanded
              * Expand Users under the database you expanded
               * Right click on "Properties" for the newly created user
                 * Provide new user with the following
                   * Check
                     * db_datareader
                     * db_datawriter
                     * db_owner
                 * Adjust any other properties for the user that is needed
                 * Click button "OK"
        * Expand the SQL server you just connected to
          * Expand Security under the server you just connected to
            * Expand on "Logins"
              * Right click on "Properties" for the newly created user
                * Select the "User Mapping" tab
                  * Select the database of choice in the "Users mapped to this login" panel
                    * Make sure the user is inputted in the User section and the Default Schema is dbo
                  * Provide new user with the following in the "Database role membership for: " section
                   * Check
                     * db_datareader
                     * db_datawriter
                     * db_owner
                * Click button "OK"

* If you have issues with deleting a user from the database then proceed with the following
  * Issue with not able to delete user because they own a database
    * Check what the user has ownership to
      * <pre>
          select
          s.name
          from sys.schemas s
          where s.principal_id = user_id('user_name');
        </pre>
    * Assign dbo to owner of the database again
      * <pre>
          ALTER AUTHORIZATION ON SCHEMA::db_owner TO dbo;
          ALTER AUTHORIZATION ON SCHEMA::db_datareader TO dbo;
          ALTER AUTHORIZATION ON SCHEMA::db_datawriter TO dbo;
        </pre>
