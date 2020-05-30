# Setting up Ubuntu on WSL for Python/Docker development

## Add Key to Github
```bash
$ cd ~/.ssh && ssh-keygen -t rsa -b 4096 -C "<email>"
$ cat id_rsa.pub
# Copy and paste this into new SSH key in Github
```

## Clone development tools
```bash
$ cd ~/ && git clone git@github.com:BradLucky/development-tools.git development-tools
```

## Install Python 3.8 
(Tips from https://linuxize.com/post/how-to-install-python-3-8-on-ubuntu-18-04/)

1. Run the following commands as root or user with sudo access to update the packages list and install the prerequisites:
```bash
$ sudo apt update
$ sudo apt install software-properties-common
```
2. Add the deadsnakes PPA to your system’s sources list:
```bash
$ sudo add-apt-repository ppa:deadsnakes/ppa
# When prompted press Enter to continue:
  [ Output ]
  Press [ENTER] to continue or Ctrl-c to cancel adding it.
```
3. Once the repository is enabled, install Python 3.8 with:
```bash
$ sudo apt install python3.8
```
4. Verify that the installation was successful by typing:
```bash
$ python3.8 --version
  [ Output ]
  Python 3.8.x
```
At this point, Python 3.8 is installed on your Ubuntu system, and you can start using it.

### Setting default Python version
This can be dangerous as it usurps the OS default Python and can prevent a variety of other things from working. The OS has an assumption of the pre-installed Python version and getting in the way of that has caused problems with `pip install` and `powerline-gitstatus` among other things. Use with caution!
(Steps based on https://unix.stackexchange.com/questions/410579/change-the-python3-default-version-in-ubuntu)
```bash
$ sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 1
# And verify
$ python3 --version
   [ Output ]
   Python 3.8.x
```
You can also point `python` to `python3`
```bash
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.8 1
```

## Install pip3
```bash
$ sudo apt install python3-pip
```

## Install libpq
This is necessary for installing `psycopg2` outside of Docker. If you're not going to be doing that, you can skip this step.
```bash
$ sudo apt-get install libpq-dev
```

## Install Powerline
```bash
$ sudo apt install powerline  # preferred over `pip3 install --user powerline-status`
```

## Symlink to development-tools
```bash
$ ln -s ~/development-tools/.bash_profile .bash_profile
```

## Set Up vim
Install Vundle
```shell
$ git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```
Link to .vimrc
```shell
$ ln -s ~/development-tools/.vimrc .vimrc
```
Install plugins
```shell
$ vim .vimrc

# Inside vim

:PluginInstall
```

## Install pytest and pylint
```bash
$ pip3 install pytest
$ pip3 install pylint
```

## Install ripgrep
The following supposedly works according to the [ripgrep GitHub page](https://github.com/BurntSushi/ripgrep), but it does not as of this writing.
```bash
$ sudo apt-get install ripgrep  # DOES NOT WORK
```
Hence, the following must be run to install this package.
```bash
$ curl -LO https://github.com/BurntSushi/ripgrep/releases/download/11.0.2/ripgrep_11.0.2_amd64.deb
$ sudo dpkg -i ripgrep_11.0.2_amd64.deb
```

## Install Node.js
You'll likely need Node.js (or at least npm) at some point, but if not, you can skip this and/or come back later.

(Tips from https://docs.microsoft.com/en-us/windows/nodejs/setup-on-wsl2#install-nvm-nodejs-and-npm)

### Install nvm (a version manager for Node.js)
```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash
```
Close your terminal and reopen it and verify it is installed.
```bash
$ command -v nvm  # expected output is 'nvm'
```

### Install the latest release of Node.js, verify versions
```bash
$ nvm install --lts
$ node --version
$ npm --version
```

## Finding files between Ubuntu and Windows
To look at distro files within Windows:
1. Press [WIN] + R (or go to the address bar in Windows Explorer)
1. Type `\\wsl$`

To open your current distro location in Windows Explorer:
```shell
$ explorer.exe .
```

## Get Docker working correctly
Follow some of the tips from here (https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly), specifically checking 'yes' to expose daemon on tcp://localhost:2375 without TLS and installing Docker and Docker Compose within WSL.

## Install GitHub CLI
https://github.blog/2020-02-12-supercharge-your-command-line-experience-github-cli-is-now-in-beta/

## Connect Git with all things in Windows
This has not been necessary thus far (thanks to WSL2, in part, and also connecting VS Code to WSL), but it seems it could be useful someday...
[How to use GIT and other Linux tools in WSL on Windows](https://medium.com/faun/how-to-use-git-and-other-linux-tools-in-wsl-on-windows-4c0bffb68b35)

## Get Selenium/ChromeDriver working in WSL
* https://github.com/lackovic/notes/tree/master/Windows/Windows%20Subsystem%20for%20Linux#use-chromedriver-on-wsl
* https://superuser.com/questions/1475553/running-selenium-on-wsl-using-chrome
