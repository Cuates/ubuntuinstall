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
    * Copy and paste starship.toml content
    * Save and exit file
* Navigate to home directory
  * `cd ~`
* Modify file .bashrc to include starship
  * `eval "$(starship init bash)"`
* Reload bash
  * `. .bashrc`
