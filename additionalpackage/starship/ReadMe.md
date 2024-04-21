[How To Install And Configure Starship On Linux](https://linuxconfig.org/how-to-install-and-configure-starship-on-linux)<br />

* Starship Download
  * `curl -O https://starship.rs/install.sh`
* Chmod File
  * `chmod +x install.sh`
* Starship Install
  * `./install.sh -b ~/bin`
    * Install Starship,  latest to /path/to/home/bin? [y/N]: y
* Navigate to home directory
  * `cd ~`
* Create config directory
  * `mkdir .config`
* Create and modify file starship.toml
  * `vim ~/.config/starship.toml`
    * Copy and paste [starship.toml](https://github.com/Cuates/windowsinstall/blob/main/system/terminal/starship.toml) content
    * Save and exit file
* Navigate to home directory
  * `cd ~`
* Modify file .bashrc to include starship
  * Get the path to starship
    * `whereis starship`
  * `vim ~/.bashrc`
    * Add the following to the bottom of the file
      * `eval "$(</path/to/starship> init bash)"`
* Reload bash
  * `. .bashrc`
