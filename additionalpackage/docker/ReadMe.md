[How to install Docker on Ubuntu 22 04](https://linuxconfig.org/how-to-install-docker-on-ubuntu-22-04)<br />
[Docker Desktop](https://www.docker.com/products/docker-desktop)<br />
[How To Install and Use Docker on Ubuntu 22 04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04)<br />
[How To Install and Use Docker Compose on Ubuntu 22 04](https://digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-22-04)<br />
[Install and Configure Docker Compose on Ubuntu 22 04 LTS Jammy](https://www.how2shout.com/linux/install-and-configure-docker-compose-on-ubuntu-22-04-lts-jammy)<br />
[Komga Windows Install and Update](https://komga.org/installation/thirdparty.html#windows-installer-and-updater)<br />
[Komga Optional COnfiguration](https://komga.org/installation/configuration.html#optional-configuration)<br />
[Komga Baseurl](https://komga.org/installation/configuration.html#server-servlet-context-path-server-servlet-context-path-baseurl)<br />
[Komga Gotson](https://github.com/gotson/komga/issues/353)<br />
[Docker Compose](https://github.com/docker/compose/releases/)<br />
[Install Docker Desktop on Ubuntu](https://docs.docker.com/desktop/install/ubuntu/)<br />
[How to Install Docker Desktop on Ubuntu 22.04](https://www.linuxtechi.com/how-to-install-docker-desktop-on-ubuntu/)<br />
[Docker Container Inspect](https://docs.docker.com/engine/reference/commandline/container_inspect/)<br />

* Docker
  * Update System
    * `sudo apt-get update`
  * Install prerequisite packages
    * `sudo apt install apt-transport-https ca-certificates curl software-properties-common`
  * Add GPG Key for the official Docker repo
    * `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`
  * Add Docker repo to APT sources
    * `echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`
  * Update existing list of packages
    * `sudo apt-get update`
  * Check if installing from Docker instead of Ubuntu
    * `apt-cache policy docker-ce`
  * Install Docker
    * `sudo apt-get install docker-ce`
  * Check system status
    * `sudo systemctl status docker`
  * Add username to the docker group
    * `sudo usermod -aG docker <username>`
      * Add any other users to the docker group if needed
  * Log out and back in for users and groups to take hold
    * NOTE: If log out does not work, then restart your system
  * Check if docker group was added
    * `groups`
* Docker Compose
  * Create directory
    * `mkdir -p ~/.docker/cli-plugins/`
  * Download docker compose
    * **NOTE** Get the latest docker compose version
    * `curl -SL https://github.com/docker/compose/releases/download/v2.16.0/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose`
  * Set permission for docker compose to be executable
    * `chmod +x ~/.docker/cli-plugins/docker-compose`
  * Verify installation was successful
    * `docker compose version`
* Docker Desktop
  * The three steps above are done before installing Docker Desktop
    * Install prerequisite packages
    * Add GPG Key for the official Docker repo
    * Add Docker repo to APT sources
  * Visit the Install Docker Desktop on Ubuntu for the latest version
    * As of this write up version is 4.16.2
  * Update System
    * `sudo apt-get update`
  * Install Package
    *  `sudo apt-get install -y ./docker-desktop-<version>-<arch>.deb`
      * i.e. `sudo apt-get install -y ./docker-desktop-4.16.2-amd64.deb`
        * Based on Docker Desktop documenation, you can ignore the warning
          * <pre>
              N: Download is performed unsandboxed as root, as file '/home/user/Downloads/docker-desktop.deb' couldn't be accessed by user '_apt'. - pkgAcquire::Run (13: Permission denied)
            </pre>
  * Open Docker Desktop GUI
    * Start Docker Desktop via GUI
    * Accept the Service Agreement, will only show the first time the app is opened after installing
  * Enable Docker Desktop to start on login
    * `sudo systemctl --user enable docker-desktop`
  * Start Docker Desktop
    * `sudo systemctl --user start docker-desktop`
  * Stop Docker Desktop
    * `sudo systemctl --user stop docker-desktop`
  * Upgrade Docker Desktop
    * `sudo apt-get install ./docker-desktop-<version>-<arch>.deb`
      * i.e. `sudo apt-get install ./docker-desktop-4.16.2-amd64.deb`
  * Remove Docker Desktop from system
    * `sudo apt purge docker-desktop`
    * `rm -r ~/.docker/desktop`
    * `sudo rm /user/local/bin/com.docker.cli`
* Docker commands
  * View all available subcommands
    * `docker`
  * View options available to a specific command
    * `docker docker-subcommand --help`
  * View system-wide information about DOcker
    * `docker info`
  * View active containers
    * `docker ps`
  * View all containers
    * `docker ps -a`
  * List containers created
    * `docker ps -l`
  * Start a container
    * `docker start <conatiner_id>/<container_name>`
      * i.e. `docker start 1c08a7a0d0e4`
      * i.e. `docker start ubuntu`
  * Stop a container
    * `docker stop <container_id>/<contatiner_name>`
      * i.e. `docker stop 1c08a7a0d0e4`
      * i.e. `docker stop ubuntu`
  * List Docker images
    * `docker images`
  * Execute bash to the Docker container
    * `docker exec -it <container_name> bash`
      * i.e. `docker exec -it nginx bash`
  * Inspect docker container
    * `docker container inspect <contianer_name>`
      * i.e. `docker container inspect nginx`
* Docker Compose commands
  * Docker compose up
    * `docker compose up -d`
  * Docker compose logs
    * `docker compose logs`
  * Pause the environment execution without changing the curren tstat of your conatiners
    * `docker compose pause`
  * Resume execution after issuing a pause
    * `docker compose unpause`
  * Terminate container execution; will not destroy any data associated with your container
    * `docker compose stop`
  * Remove conatiner; will remove the containters, networks, and volumes assocaited with this containerized environment
    * `docker compose down`
  * Remove base image
    * `docker image rm <docker_image_container_name>`
      * i.e. `docker image rm nginx:alpine`
