**VirtualBox quick start guide**
- Download VirtualBox
 - https://www.virtualbox.org/wiki/Testbuilds
- Download Ubuntu
 - http://www.ubuntu.com/download/desktop
- Install Ubuntu on VirtualBox
- Go to ‘Devices’ menu and install ‘Guest additions’ to get proper display resolution (restart required)
- Make VB network available as bridged
- Click the network icon (two computers) -> “network settings” -> “Attached to” -> “Bridged adapter”
- Update all stuff on VB
 - `sudo apt-get update`
 - `sudo apt-get upgrade`
- Create user for using SSH on VB
 - `sudo adduser calleremote # create user calleremote`
 - `sudo gpasswd -a calleremote sudo # make calleremote a super user`
 - `ssh-keygen # create public and private keys`
- install GIT `apt-get install git`

- SSH server for VB
 - https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04
- sudo apt-get install openssh-server
- Get public key to Host
 - Host `brew install ssh-copy-id`
 - Host `ssh-copy-id calleremote@198.168.1.67 # ip to VBox`
 - Host `ssh calleremote`

**VirtualBox applications**
- Install Node on VB
 - https://github.com/joyent/node/wiki/installing-node.js-via-package-manager
- StrongLoop
 - npm install -g strongloop
- PM2
- npm install -g pm2
