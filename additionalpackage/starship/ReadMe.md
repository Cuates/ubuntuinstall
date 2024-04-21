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
* Navigate into config directory
  * `cd ~/.config`
* Create file starship.toml
  * `vim starship.toml`
    * Copy and paste [starship.toml](https://github.com/Cuates/windowsinstall/blob/main/system/terminal/starship.toml) content
    * Save and exit file
* Navigate to home directory
  * `cd ~`
* Modify file .bashrc to include starship
  * `vim ~/.bashrc`
    * Add the following to the bottom of the file
      * `eval "$(starship init bash)"`
* Reload bash
  * `. .bashrc`
