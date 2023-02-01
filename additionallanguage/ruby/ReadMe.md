[Install Ruby Ubuntu](https://phoenixnap.com/kb/install-ruby-ubuntu)<br />

* Update
  * `sudo apt-get update`
* Install Dependencies
  * `sudo apt-get install -y git curl autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev`
* Download and Run Shell Script
  * `curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash`
* Add rbenv to your PATH environment variable
  * `echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc`
  * `echo 'eval "$(rbenv init -)"' >> ~/.bashrc`
  * `source ~/.bashrc`
* Check rbenv Version
  * `rbenv -v`
* List Ruby Versions
  * `rbenv install -l`
* Install Ruby
  * `rbenv install 3.2.0`
    * **WAIT FOR THIS TO FINISH**
* Set Ruby as Global Version
  * `rbenv global 3.0.2`
* Check Ruby Version
  * `ruby --version`
