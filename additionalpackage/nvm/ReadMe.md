[nvm](https://github.com/nvm-sh/nvm)<br />

* Installing Node Version Manager (NVM)
  * Go to the github repository to get the latest version
    * Execute the following line with the version you download from github
      * `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash`
  * Close and reopen terminal or source your profile
    * This will install the nvm script to your user account.
  * To verify that nvm has been installed, do:
    * `command -v nvm`
      * which should output nvm if the installation was successful
  * You can list available versions using ls-remote:
    * `nvm ls-remote`
  * Install Nodejs version of choice
    * Install the latest version using one of the following commands below. NOTE: Your Nodejs version might be different with what is shown below.
      * `nvm install v18.13.0`
      * `nvm install lts/hydrogen`
  * You can verify that the install was successful using the same technique from the other sections, by typing:
    * `node --version`
  * Uninstalling Node Version Manager (NVM)
    * If you wish to uninstall them at a later point (or re-install them under your 'nvm' Nodes), you can remove them from the system Node as follows:
      * Execute the following command to remove nvm from your system
        * `nvm use system`
        * `npm uninstall -g a_module`
