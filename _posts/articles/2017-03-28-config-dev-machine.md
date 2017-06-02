---
layout: post
title: Configure your dream macOS dev machine
excerpt: "How to configure your hackintosh for Webdev"
<!-- modified: 2016-06-01T14:17:25-04:00 -->
categories: articles
tags: [development, Mac OS]
author: Panagiotis Sebos
image:
  feature: so-simple-sample-image-1.jpg
  <!-- credit: WeGraphics -->
  <!-- creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/ -->
comments: true
share: false
---

Configure you dream sierra dev machine

**Revision 0** updated 02/2016 for 10.11 El Capitan

**Revision 1** updated 10/2016 for 10.12 Sierra

**Revision 2** updated 01/2017 for 10.12.2 Sierra with new i7 bog

**sublime hot tip:** User CMD+P and then @ on this document and it will list the headers of the markdown

**rouge highlighter:** supported languages can be found [here](https://github.com/jneen/rouge/wiki/List-of-supported-languages-and-lexers)

:skull:, :zap:, :+1: , :bowtie:  and :hand:, now with [emoji support](https://github.com/jekyll/jemoji). Check the [cheatsheet](http://www.emoji-cheat-sheet.com/)

* This line is a placeholder to generate the table of contents
{:toc}

> Various software used for installing macOS in all machines can be found at `{{site.software_location}}`. Backup files can be found in `{{site.backup_location}}` and specific hardware configuration files are located in `{{site.sixty_location}}`.

> All licenses and files can be found in `/Dropbox/apps osx`

### Printers and Scanners ###
[up up up](#)

#### wks-0088-25 ####

* `Add > IP > prt-0088-03.bankofgreece.gr`

```plaintext
HP Color LasetJet 5550 DTN
Duplex Unit
Nearest Size & Scale
```

* `Add > IP > prt-0088-01.bankofgreece.gr`

```plaintext
HP LaserJect 8150 Series
Duplex Unit
Envelope Feeder
```



### System Preferences Configuration ###
[up up up](#)

* Mouse > Scroll direction: Natural **unchecked**
* Displays > Show mirroring option in the menu bar when available **unchecked**
* Sound > Show volume in menu bar **checked**
* Language & Region > Add Greek
* Keyboard > Modifier Keys **switch alt and command**
* Keyboard > Shortcuts > Spotlight > Show Spotlight search > ^Space
* Keyboard > Shortcuts > Input Sources > Select previous input source > <CMD>Space
* Keyboard > Shortcuts > Input Sources > Select next input source **unchecked**
* iCloud > Login
* iCloud > Use iCloud for Mail...
* iCloud > Use Find my Mac
* iCloud > Contacts **unchecked**
* iCloud > KeyChain **unchecked**
* Internet Accounts > Facebook
* Internet Accounts > Facebook > Calendars **unchecked**
* Internet Accounts > Google
* Internet Accounts > Google > mail **unchecked**
* Internet Accounts > Google > messages **unchecked**
* Internet Accounts > Google > notes **unchecked**

* Keychain Access > Preferences > General > Show keychain status in menu bar **checked**

### Disable VNC bind on ethernet Interface ###

```shell
sudo defaults write /Library/Preferences/com.apple.RemoteManagement.plist VNCOnlyLocalConnections -bool yes
```

and restart screen sharing. Kudos goes to [this](http://apple.stackexchange.com/questions/216689/how-to-configure-mac-screen-sharing-to-only-listen-on-localhost)

### Update sshd ####
[up up up](#)

** only in wks-0088-25 **

enable sshd .ssh.local
`/etc/ssh/sshd_config`

```conf
AuthorizedKeysFile      .ssh.local/authorized_keys

# sudo launchctl unload /System/Library/LaunchDaemons/ssh.plist
# sudo launchctl load /System/Library/LaunchDaemons/ssh.plist
```

** in all machines **

```conf
# psebos hardening
# https://stribika.github.io/2015/01/04/secure-secure-shell.html
Protocol 2
HostKey /etc/ssh/ssh_host_ed25519_key
HostKey /etc/ssh/ssh_host_rsa_key
PasswordAuthentication no
ChallengeResponseAuthentication no
PubkeyAuthentication yes
KexAlgorithms curve25519-sha256@libssh.org,diffie-hellman-group-exchange-sha256
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-ripemd160-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-512,hmac-sha2-256,hmac-ripemd160,umac-128@openssh.com
```

### Basic Software Installation ###
[up up up](#)

Install standalone:

* [Dropbox](https://www.dropbox.com/) *Download, Let it sync and rename device*
* [Resilio Sync](https://getsync.com/individuals/) *Download, Let it sync and rename device*

in the Resilio Sync folder get into the `.sync` directory and check the `IgnoreList` file. I had to do the following in order to sync `pubring.gpg~` files:

```shell_session
# https://help.getsync.com/hc/en-us/articles/205458165-Ignoring-files-in-Sync-Ignore-List-
# psebos pubring.gpg~
#*~
```

* [Chrome](https://www.google.com/chrome/)
* [Thunderbird](https://www.mozilla.org/en-US/thunderbird/)
* [Firefox Developer Edition](https://www.mozilla.org/en-US/firefox/developer/)
* [Firefox](https://www.mozilla.org/en-US/firefox/new/)
* [1Password](https://www.agilebits.com/)
* [Sublime Text 3](https://www.sublimetext.com/3)
* [iTerm2 3.0.13 (OS 10.8+)](https://www.iterm2.com/downloads.html)
* SSH Tunnel (AppStore)
* AirMail (AppStore)
* VOX (AppStore)
* Aja System Test (AppStore)
* Shazam (AppStore)
* Pocket (AppStore)
* Xcode (AppStore)
* Twitter (AppStore)
* The Unarchiver (AppStore)
* Microsoft Remote Desktop (AppStore)

**sync all cloud services**

and setup links:

```shell_session
cd ~
ln -s /Volumes/data/Bit

sudo su -
cd /
ln -s /Volumes/data
ln -s /Users/psebos/Dropbox
ln -s /Volumes/data/Bit
ln -s /Users/psebos/Desktop
ln -s /Users/psebos/Documents
ln -s /Users/psebos/Downloads
chmod -h 755 /Dropbox
chown -h psebos:staff /Dropbox
chmod -h 755 /Bit
chown -h psebos:staff /Bit
chmod -h 755 /Desktop
chown -h psebos:staff /Desktop
chmod -h 755 /Documents
chown -h psebos:staff /Documents
chmod -h 755 /Downloads
chown -h psebos:staff /Downloads
```

### Setup sudo ###
[up up up](#)

```shell_session
sudo cp /etc/sudoers /etc/sudoers_
sudo visudo

# add @ the end
# psebos
psebos  ALL=(ALL) NOPASSWD: ALL
```

### Setup brew ###
[up up up](#)

install proxy if needed

```shell_session
PROXY='10.130.22.135'

cat >~/.wgetrc <<EOF
https_proxy = http://${PROXY}:3128/
http_proxy = http://${PROXY}:3128/
ftp_proxy = http://${PROXY}:3128/
# If you do not want to use proxy at all, set this to off.
use_proxy = on
EOF

cat >~/.curlrc <<EOF
proxy = "${PROXY}:3128"
EOF

cat >~/.gitconfig <<EOF
[http]
  proxy = socks5://${PROXY}:7070
[user]
  name = sakoula
  email = psebos@gmail.com
[github]
  token = 340182628c7feed48738a2ff7a2b252264f55d65
EOF

cat >~/.gemrc <<EOF
---
http_proxy: http://${PROXY}:3128
EOF
```



install brew

```shell_session
xcode-select --install # install xcode command line tools
sudo xcodebuild -license

ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
cd /usr/local/bin
brew doctor
cd /usr/local/bin
ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl
```

```shell_session
xcode-select --install # install xcode command line tools

/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
cd /usr/local/bin
brew doctor
cd /usr/local/bin
ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl
```

install various apps

```shell_session
#  •  Silver Searcher, ack (via brew) and spot
curl -L https://raw.github.com/guille/spot/master/spot.sh -o /usr/local/bin/spot && chmod +x /usr/local/bin/spot
brew update
# Install GNU core utilities (those that come with OS X are outdated)
brew install coreutils
# Install GNU `find`, `locate`, `updatedb`, and `xargs`, g-prefixed
brew install findutils
# Install Bash 4
brew install bash
brew install bash-completion
brew tap homebrew/completions
# Install more recent versions of some OS X tools
brew tap homebrew/dupes
brew install homebrew/dupes/grep
mv $(brew --prefix coreutils)/libexec/gnubin/ls $(brew --prefix coreutils)/libexec/gnubin/gls
mv $(brew --prefix coreutils)/libexec/gnubin/stty $(brew --prefix coreutils)/libexec/gnubin/gstty # for the arduino
```

change default shell

```shell_session
#change default bash
#http://apple.stackexchange.com/questions/24632/is-it-safe-to-upgrade-bash-via-homebrew
Adding /usr/local/bin/bash to /etc/shells
Running chsh -s /usr/local/bin/bash.
```

install various apps with brew

```shell_session
brew install tinyproxy
brew services start tinyproxy
brew install nginx
brew services start nginx

apps=(
 ack
 android-platform-tools
 autoconf
 automake
 azure-cli
 bash
 bash-completion
 beanstalk
 beanstalkd
 boost
 coreutils
 ctags
 dirmngr
 dockviz
 dos2unix
 fcgi
 fcgiwrap
 ffmpeg
 findutils
 flac
 fontconfig
 freetype
 gd
 geoip
 gettext
 glib
 gnupg
 gnupg2
 gpg-agent
 graphviz
 grep
 highlight
 htop
 id3lib
 jpeg
 lame
 libassuan
 libevent
 libffi
 libgcrypt
 libgpg-error
 libiconv
 libksba
 libpng
 libtiff
 libtool
 libusb
 libusb-compat
 lua
 nmap
 openssl
 packer
 pcre
 pinentry
 pkg-config
 pth
 pwgen
 rbenv
 readline
 ruby-build
 scrub
 shellcheck
 socat
 ssh-copy-id
 stunnel
 the_silver_searcher
 tidy-html5
 tmux
 unar
 unrar
 utf8proc
 webp
 wget
 x264
 xvid
 xz
 youtube-dl
 exiftool
)
brew install ${apps[@]}
brew install smartmontools
brew install bonnie++

$ comm -2 -3  file wks-0088-25
```

install apps with cask

```shell_session
# cask does not require separate installation anymore
# http://sourabhbajaj.com/mac-setup/
# https://github.com/sindresorhus/quick-look-plugins
brew cask search /google-chrome/

# you may choose not to install these
brew cask install java
brew cask install genymotion
brew cask install flash-npapi

brew cask install android-file-transfer
brew cask install appcleaner
brew cask install arduino
brew cask install betterzipql
brew cask install cheatsheet
brew cask install daisydisk
brew cask install docker
brew cask install docker-toolbox
brew cask install geekbench
brew cask install intel-power-gadget
brew cask install fritzing
brew cask install launchcontrol
brew cask install moom
brew cask install dash
brew cask install kodi
brew cask install nvalt
brew cask install postman
brew cask install qlcolorcode
brew cask install qlimagesize
brew cask install qlmarkdown
brew cask install qlprettypatch
brew cask install qlstephen
brew cask install quicklook-csv
brew cask install quicklook-json
brew cask install soapui
brew cask install showyedge
brew cask install skype
brew cask install spotify
brew cask install sqlitebrowser
brew cask install suspicious-package
brew cask install teamviewer
brew cask install vagrant
brew cask install vagrant-manager
brew cask install virtualbox
brew cask install virtualbox-extension-pack
brew cask install vlc
brew cask install vmware-fusion
brew cask install webpquicklook
brew cask install xquartz
brew cask install timemachineeditor

brew cask install steam
brew cask install wireshark
```

prior continue get rid off the daemons of teamviewer:

```shell_session
cd /Library/LaunchDaemons
sudo rm com.teamviewer.*

cd /Library/LaunchAgents
sudo rm com.teamviewer.teamviewer*
```


install ntfs/sshfs/ftpfs support

```shell_session
# upgrade
brew tap homebrew/fuse # for sshfs etc
brew update
brew upgrade
cd /sbin
sudo mv mount_ntfs mount_ntfs_
brew cask install osxfuse # cask programs should be upgraded like this
brew install homebrew/fuse/ntfs-3g
brew install homebrew/fuse/sshfs
brew install homebrew/x11/curlftpfs

cd /sbin
sudo ln -s /usr/local/bin/sshfs /sbin/mount_sshfs
sudo ln -s /usr/local/sbin/mount_ntfs /sbin/mount_ntfs
sudo ln -s /usr/local/bin/curlftpfs /sbin/mount_ftpfs
```

install npm

```shell_session
# how2 read file while is changing
npm install -g how2
```

#### setup fstab ####

```shell_session
add/change the last 3 lines @ the end of /etc/auto_master
# psebos
# /Network/Servers      -fstab
/mnt                    -null
/-                      -static

this is the problem with taking much time to shutdown etc
/Network/Servers -fstab
```

#### fstab wks-0088-25 ####

```shell_session
sudo mkdir /mnt
sudo mkdir /mnt/centos
sudo mkdir /mnt/debian
sudo mkdir /mnt/gourouni
sudo mkdir /mnt/ifgw
sudo mkdir /mnt/infinit
sudo mkdir /mnt/koo
sudo mkdir /mnt/mfgw
sudo mkdir /mnt/perseas
sudo mkdir /mnt/sagt
sudo mkdir /mnt/saperpdev
sudo mkdir /mnt/saperpprd
sudo mkdir /mnt/saperpqas
sudo mkdir /mnt/sappro
sudo mkdir /mnt/srv-0088-04
sudo mkdir /mnt/srv-amlcmt-dev
sudo mkdir /mnt/srv-amlcmt-prd
sudo mkdir /mnt/srv-amlweb-dev
sudo mkdir /mnt/srv-amlweb-prd
sudo mkdir /mnt/srv-dlt-01
sudo mkdir /mnt/srv-dlt-02
sudo mkdir /mnt/srv-ifgw-dev
sudo mkdir /mnt/srv-ifgw-pro
sudo mkdir /mnt/srv-ifgw-tst
sudo mkdir /mnt/srv-iris-dev
sudo mkdir /mnt/srv-irisapp-pro1
sudo mkdir /mnt/srv-irisapp-pro2
sudo mkdir /mnt/srv-irisweb-dev
sudo mkdir /mnt/srv-irisweb-pro1
sudo mkdir /mnt/srv-irisweb-pro2
sudo mkdir /mnt/srv-log-dev
sudo mkdir /mnt/srv-mfgw-dev
sudo mkdir /mnt/srv-mfgw-pro
sudo mkdir /mnt/srv-mfgw-tst
sudo mkdir /mnt/srv-ora-db1
sudo mkdir /mnt/srv-ora-db2
sudo mkdir /mnt/srv-ora-drs
sudo mkdir /mnt/srv-saperp-predev
sudo mkdir /mnt/srv-tekessh-prd1
sudo mkdir /mnt/srv-tekeweb-prd1
sudo mkdir /mnt/sysm
sudo mkdir /mnt/sysp
sudo mkdir /mnt/wks-0088-26
sudo mkdir /mnt/wks-0088-26e
sudo mkdir /mnt/srv-0088-01
sudo mkdir /mnt/srv-0088-01/0088
sudo mkdir /mnt/srv-0088-01/Drivers
sudo mkdir /mnt/srv-0088-01/ISO-Repository
sudo mkdir /mnt/srv-sapbw-pro
sudo chown -R psebos:staff /mnt
sudo chmod -R 700 /mnt
```

```conf
sudo vifs
#
# Warning - this file should only be modified with vifs(8)
#
# Failure to do so is unsupported and may be destructive.
#
## sshfs
# for options check man mount, man sshfs
# IMPORTANT: make sure that you have a symbolic link in h
root@centos:/ /mnt/centos sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@gourouni:/ /mnt/gourouni sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@perseas:/ /mnt/perseas sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
psebos@koo:/ /mnt/koo sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-ora-drs:/ /mnt/srv-ora-drs sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-ora-db1:/ /mnt/srv-ora-db1 sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-ora-db2:/ /mnt/srv-ora-db2 sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-0088-04:/ /mnt/srv-0088-04 sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-mfgw-dev:/ /mnt/srv-mfgw-dev sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@mfgw:/ /mnt/mfgw sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-ifgw-dev:/ /mnt/srv-ifgw-dev sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@ifgw:/ /mnt/ifgw sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
bog@srv-mfgw-tst:/ /mnt/srv-mfgw-tst sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
bog@srv-ifgw-tst:/ /mnt/srv-ifgw-tst sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
bog@srv-mfgw-pro:/ /mnt/srv-mfgw-pro sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
bog@srv-ifgw-pro:/ /mnt/srv-ifgw-pro sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@saperpdev:/ /mnt/saperpdev sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@saperpqas:/ /mnt/saperpqas sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@saperpprd:/ /mnt/saperpprd sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@sappro:/ /mnt/sappro sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@sagt:/ /mnt/sagt sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-saperp-predev:/ /mnt/srv-saperp-predev sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-amlcmt-dev:/ /mnt/srv-amlcmt-dev sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-amlcmt-prd:/ /mnt/srv-amlcmt-prd sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-amlweb-dev:/ /mnt/srv-amlweb-dev sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-amlweb-prd:/ /mnt/srv-amlweb-prd sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-irisweb-dev:/ /mnt/srv-irisweb-dev sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-irisweb-pro1:/ /mnt/srv-irisweb-pro1 sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-irisweb-pro2:/ /mnt/srv-irisweb-pro2 sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-irisapp-pro1:/ /mnt/srv-irisapp-pro1 sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-irisapp-pro2:/ /mnt/srv-irisapp-pro2 sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-iris-dev:/ /mnt/srv-iris-dev sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-log-dev:/ /mnt/srv-log-dev sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@debian:/ /mnt/debian sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-dlt-01:/ /mnt/srv-dlt-01 sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-dlt-02:/ /mnt/srv-dlt-02 sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
Administrator@srv-tekeweb-prd1:/ /mnt/srv-tekeweb-prd1 sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@srv-tekessh-prd1:/ /mnt/srv-tekessh-prd1 sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
ftpusr1@sysp:/ /mnt/sysp sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
ftpusr1@sysm:/ /mnt/sysm sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
## cifs
# for options check man mount, man mount_smbfs
//psebos@srv-0088-01/0088 /mnt/srv-0088-01 smbfs soft 0 0
//BOGNET;psebos@srv-0088-01/0088 /mnt/srv-0088-01/0088 smbfs rw,soft 0 0
//BOGNET;psebos@srv-0088-01/Drivers /mnt/srv-0088-01/Drivers smbfs rw,soft 0 0
//BOGNET;psebos@srv-0088-01/ISO-Repository /mnt/srv-0088-01/ISO-Repository smbfs rw,soft 0 0
//BOGNET;psebos@wks-0088-26/C$ /mnt/wks-0088-26 smbfs rw,soft 0 0
//BOGNET;psebos@wks-0088-26/E$ /mnt/wks-0088-26e smbfs rw,soft 0 0
//BOGNET;psebos@srv-sapbw-pro/D$ /mnt/srv-sapbw-pro smbfs rw,soft 0 0
```

#### fstab sixty ####

```shell_session
sudo mkdir /mnt
sudo mkdir /mnt/gourouni
sudo mkdir /mnt/perseas
sudo mkdir /mnt/debian
```

```conf
sudo vifs
#root@gourouni:/ /mnt/gourouni sshfs user,noauto,exec,ServerAliveInterval=15,ServerAliveCountMax=3 0 0
#root@gourouni:/ /mnt/gourouni sshfs allow_other,auto_cache,reconnect,follow_symlinks,volname="Distant folder" 0 0
root@perseas:/ /mnt/perseas sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@debian:/ /mnt/debian sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
root@gourouni:/ /mnt/gourouni sshfs reconnect,follow_symlinks,ServerAliveInterval=15,ServerAliveCountMax=3,allow_other,uid=501,gid=20 0 0
```

```shell_session
# important!!!!!
sudo chmod go+r /etc/fstab
sudo chown psebos:staff /mnt
sudo chown psebos:staff /mnt/*
```

### iTerm and Shell Syncing ###
[up up up](#)

setup variables

```shell_session
BD='{{site.backup_location}}'
HOST='sixty'

or

BD='{{site.backup_location}}'
HOST='wks-0088-25'

BD='{{site.backup_location}}'
HOST='koo'
```

```shell_session
cd ~
mv .bash_profile .bash_profile_
mv .bashrc .bashrc_
mv .ssh .ssh_
mv .vimrc .vimrc_
ln -s /Dropbox/sync/${HOST}/Users/psebos/.bash_profile
ln -s /Dropbox/sync/${HOST}/Users/psebos/.bashrc
ln -s /Dropbox/sync/${HOST}/Users/psebos/.ssh
ln -s /Dropbox/sync/${HOST}/Users/psebos/.vimrc
ln -s /Dropbox/sync/${HOST}/Users/psebos/.rubocop
ln -s /Dropbox/sync/${HOST}/Users/psebos/.html.tidy.conf
ln -s /Dropbox/sync/${HOST}/Users/psebos/.eslintrc
ln -s /Dropbox/sync/${HOST}/Users/psebos/.msmtprc
ln -s /Dropbox/sync/${HOST}/Users/psebos/.mailrc
ln -s /Dropbox/sync/${HOST}/Users/psebos/.mailrc
ln -s /Dropbox/sync/${HOST}/Users/psebos/.pryrc
```

additional for the proxy settings:

```shell_session
cd ~
ln -s /Dropbox/sync/${HOST}/Users/psebos/.curlrc
ln -s /Dropbox/sync/${HOST}/Users/psebos/.wgetrc
ln -s /Dropbox/sync/${HOST}/Users/psebos/.gemrc
ln -s /Dropbox/sync/${HOST}/Users/psebos/.gitconfig
```


```shell_session
cd ~/Library/Preferences
rm com.googlecode.iterm2.plist
ln -s /Dropbox/sync/Library/Preferences/com.googlecode.iterm2.plist

cd ~
curl -L https://iterm2.com/misc/`basename $SHELL`_startup.in >> \
~/.iterm2_shell_integration.`basename $SHELL`

open /Dropbox/sync/${HOST}/Users/psebos/iterm_colors.itermcolors

sudo su -
cd /var/root
cp ${BD}/var/root/.profile .
```

open iTerm

* Preferences > Keys > Show/hide iTerm2 with a system-wide hotkey **checked**
* Preferences > Keys > Hotkey toggles a dedicated window with profile **checked**
* Preferences > Profiles > Colors > Default > Color_Presets > iterm_colors
* Preferences > Profiles > Colors > Hotkey > Color_Presets > iterm_colors

### certificates ###
[up up up](#)

All certificates are in windows store and in AD.

for the personal certificate check the 1Password or nvalt

In your bog computer add the following certificates in keychain > login > Certificates  as depicted in `/Dropbox/hacking/ssl_certificates/howto_certificates.png` :

* `/Dropbox/hacking/ssl_certificates/BankofGreece_RootCA_Public_Key_2030.cer`
* `/Dropbox/hacking/ssl_certificates/BankofGreece_SubCA2.cer`
* `/Dropbox/hacking/ssl_certificates/grpsebos.escb.private.p12`
* `/Dropbox/hacking/ssl_certificates/grpsebos.escb.certificate.cer`
* `/Dropbox/hacking/ssl_certificates/ECB.rootCA.cer`
* `/Dropbox/hacking/ssl_certificates/ECB.subCA.cer`

in all machines add this:

* `/Dropbox/hacking/ssl_certificates/star.banan.es.selfsigned.cer`

### rbenv ###
[up up up](#)

check [command reference](https://github.com/rbenv/rbenv#command-reference)

uninstall rvm by doing `rvm implode`

```shell_session
brew update
brew install rbenv

#Rbenv stores data under ~/.rbenv by default. If you absolutely need to
#store everything under Homebrew's prefix, include this in your profile:
#  export RBENV_ROOT=/usr/local/var/rbenv
#
#To enable shims and autocompletion, run this and follow the instructions:
#  rbenv init

$ rbenv init

# Load rbenv automatically by appending
# the following to ~/.bash_profile:

eval "$(rbenv init -)"

# list all available versions:
$ rbenv install -l

# install a Ruby version:
$ rbenv install 2.4.0

# Sets the global version of Ruby to be used in all shells by writing the version name to the ~/.rbenv/version file
$ rbenv global 2.4.0

# check your version
$ rbenv version

# do a rehash after installing a new ruby
$ rbenv rehash

# check where the gems will be installed
$ gem env home

# upgrade ruby
$ rbenv install 2.4.1
$ rbenv global 2.4.1
# do a rehash after installing a new ruby
$ rbenv rehash
# check where the gems will be installed
$ gem env home
# The ruby-build plugin provides an rbenv uninstall command to automate the removal process.
$ rbenv uninstall 2.4.0
# install the basic gems to start with
gem install pry-byebug
gem install rubocop
gem install bundler
```


### rvm ###
[up up up](#)

```shell_session
proxy_on
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
\curl -sSL https://get.rvm.io | bash -s stable

$ Adding rvm PATH line to /Users/psebos/.profile /Users/psebos/.mkshrc /Users/psebos/.bashrc /Users/psebos/.zshrc.
$ Adding rvm loading line to /Users/psebos/.profile /Users/psebos/.bash_profile /Users/psebos/.zlogin.
$* To start using RVM you need to run `source /Users/psebos/.rvm/scripts/rvm`
$    in all your open shell windows, in rare cases you need to reopen all shell windows.

# add to .bash_profile

# rvm
PATH=$PATH:$HOME/.rvm/bin # Add RVM to PATH for scripting
[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" # Load RVM into a shell session *as a function*
# rvm use / install [tab] [tab]
[[ -r $rvm_path/scripts/completion ]] && . $rvm_path/scripts/completion

proxy_on
rvm list known # list known rubies
rvm notes # check readme
rvm requirements # install requirements
rvm list known
rvm install 2.0.0 # install 2.0.0 from known
rvm docs generate # install docs
rvm -v # version running
rvm use 2.0.0 # use installed rvm ruby
rvm system # switch back to ruby
rvm list # currently installed
rvm info # print info of the currently used ruby
rvm --default use 2.0.0
rvm [use] system
rvm [use] 2.0.0
rvm [use] default

rvm install ruby-2.2-head

proxy_on
gem install pry
gem list # list local gem setup
gem environment # list gems environment
gem update # upgrade all gems
gem cleanup --dryrun
gem cleanup rjb # remove all old versions of the gem

# install heroku toolbelt (standalone)
https://toolbelt.heroku.com/
wget -qO- https://toolbelt.heroku.com/install.sh | sh
sudo chown psebos /usr/local/heroku
added in .bash_profile
export PATH="/usr/local/heroku/bin:$PATH"


# .bash_profile is executed for login shells (console, ssh, iterm|terminal, su - psebos)
# .bashrc is executed for interactive non-login shells (xterm, run /bin/bash inside bash, su psebos)
#/bin/bash scripts run in their parent environment
#.profile .login is not used for mac but in other systems for login
# http://www.joshstaiger.org/archives/2005/07/bash_profile_vs.html
# http://stackoverflow.com/questions/415403/whats-the-difference-between-bashrc-bash-profile-and-environment


git clone ssh://git@bitbucket.org/golden_drivers/bog_dickus bog_dickus # is the same as follows
cd /Copy/dev/25-BogDickusSinatraBestPractices
git remote -v
# change stuff
git add .
git commit
git push origin master
git pull origin master
git config heroku.remote heroku # run on heroku with heroku env
git push heroku master
heroku ps
heroku ps:scale web=0 # stop app
heroku ps:scale web=1 # start app
heroku logs
heroky open
heroku run bash
heroku domains:add bog_dickus.banan.es
CNAME heroku fierce-refuge-7598.herokuapp.com # domaindiscount24

# create piggybacking ssl
# - create new app from web interface
# - add in .git/config
# [remote "heroku"]
#   url = https://git.heroku.com/thawing-forest-9956.git
#   fetch = +refs/heads/*:refs/remotes/heroku/*
#   remote = heroku
# or
# heroku git:remote -a thawing-forest-9956

git push heroku master
heroku ps --app thawing-forest-9956
heroku ps:scale web=0 --app thawing-forest-9956 # stop app
heroku ps:scale web=1 --app thawing-forest-9956 # start app
heroku logs --app thawing-forest-9956
heroku open --app thawing-forest-9956
heroku run bash --app thawing-forest-9956
heroku domains:add bog_dickus.banan.es --app thawing-forest-9956
#CNAME bog_dickus thawing-forest-9956.herokuapp.com # cloudfare
#check https://www.cloudflare.com/a/crypto/banan.es SSL full


#deploy:
#copy bog_dickus to srv-0088-04:/usr/share/nginx/bog_dickus.old
ssh -R 3128:127.0.0.1:3128 root@srv-0088-04
/etc/init.d/cntlm stop
cd /usr/share/nginx
chown -R www-data:www-data bog_dickus.old
cd /usr/share/nginx/bog_dickus.old
mkdir -p tmp/pids
mkdir -p tmp/sockets
chown -R www-data:www-data tmp
bundle update
/etc/init.d/thin stop
/etc/init.d/thin start
# test, if ok https://srv-0088-04:1414/search
/etc/init.d/thin stop
/etc/init.d/thin.2.2.1 stop
mv bog_dickus.old bog_dickus.old_
mv bog_dickus bog_dickus.old
mv bog_dickus.old_ bog_dickus
/etc/init.d/thin start
/etc/init.d/thin.2.2.1 start

git log --follow -- views/login.erb # show commit history of file login.erb even if it was renamed
git show 7726818baa59d2e80ce9c764ace8865d9f735e59:views/login.erb >./views/login.old.erb
```

### Setup Sublime Text 3 ###
[up up up](#)

* install sublime text 3
* install hack font from `/Dropbox/sync/Sublime Text 3`
* install package control quit sublime

```shell_session
cd ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/
rm -r User
ln -s /Users/psebos/Dropbox/sync/Sublime\ Text\ 3/User

cd /usr/local/bin
ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl
```

**Launch sublime and wait for all packages to get installed locally**

**if required install manually PyV8 (for bognet) from [here](https://github.com/emmetio/pyv8-binaries)**


quit sublime and do the following:

```shell_session
install package control

# this is old
# brew install node@4
# brew unlink node@4
# brew install homebrew/versions/node4-lts

#uninstall all nodejs modules with
npm ls -gp --depth=0 | awk -F/node_modules/ '{print $2}' | grep -vE '^(npm|)$' | xargs -r npm -g rm
brew uninstall node # and uninstall all of them
mv /usr/local/lin/node_modeles /Downloads/

brew install node
npm install -g browser-sync

# as in
#https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb
(
  export PKG=eslint-config-airbnb;
  npm info "$PKG@latest" peerDependencies --json | command sed 's/[\{\},]//g ; s/: /@/g' | xargs npm install --save-dev "$PKG@latest"
)

npm install -g csslint
npm install -g write-good
brew install shellcheck
gem install rubocop
brew install tidy-html5
npm install -g --save-dev eslint-config-angular

if using rbenv enable:
Tools → Build System → New Build System:

{
  "cmd": ["ruby", "$file"],
  "file_regex": "^(...*?):([0-9]*):?([0-9]*)",
  "selector": "source.ruby",
  "path": "$HOME/.rbenv/shims:$PATH"
}

save it as Ruby rbenc.build-system


install
SublimeLinter-shellcheck
SublimeLinter-rubocop
SublimeLinter-eslint
SublimeLinter-html-tidy
SublimeLinter-csslint
SublimeLinter-contrib-write-good

remap emmet ctrl+e and ctrl+u
https://github.com/sergeche/emmet-sublime#overriding-keyboard-shortcuts

add these in the emmet user settings
{
  "disabled_keymap_actions": "expand_abbreviation, update_image_size"
}


http://www.johninux.com/sublime/2013/12/30/the-best-plugin-in-sublime.html
https://ruby-china.org/topics/22554
check on this for readme
http://sublimecodeintel.github.io/SublimeCodeIntel/
SublimeCodeIntel
add this in ~/.codeintel/config

{
    "PHP": {
        "php": '/usr/bin/php',
        "phpExtraPaths": [],
        "phpConfigFile": 'php.ini'
    },
    "JavaScript": {
        "javascriptExtraPaths": []
    },
    "Perl": {
        "perl": "/usr/bin/perl",
        "perlExtraPaths": []
    },
    "Ruby": {
//        "ruby": "~/.rvm/rubies/default",
//        "rubyExtraPaths": ["~/.rvm/gems/ruby-2.2.0"]
        "ruby": "`/usr/local/bin/rbenv which ruby` \"$@\"",
        "rubyExtraPaths": []
    },
    "Python": {
        "python": '/usr/bin/python',
        "pythonExtraPaths": []
    },
    "Python3": {
        "python": '/usr/bin/python3',
        "pythonExtraPaths": []
    }
}

add this to ~/.eslintrc

// Use this file as a starting point for your projects .eslintrc.
// Copy this file, and add rule overrides as needed.
{
  "extends": ["eslint:recommended", "angular"],
  "rules": {
    // check rules
    // https://github.com/dustinspecker/eslint-config-angular/blob/master/index.js
    'angular/controller-name': [2, '/[A-Z].*Ctrl$/'],
    'angular/controller-as-vm': [2, 'self']
  }
}

open a ruby file and Sublime Text → Preferences → Settings – More → Syntax Specific – User

To tell Sublime to include those trailing ? and ! characters in word selections, add the following property to the Ruby syntax-specific settings. This has a side-effect of improving the Dash ⌃ ctrlh behavior as well!


{
  "word_separators": "./\\()\"'-:,.;<>~@#$%^&*|+=[]{}`~"
}

check this
https://mattbrictson.com/sublime-text-3-recommendations
```


### Various ###
[up up up](#)

```shell_session
# to enable airdrop on ALL macs do this and REBOOT
defaults write com.apple.NetworkBrowser BrowseAllInterfaces 1
killall Finder
```

### Moom ###
[up up up](#)

install `brew cask install moom`

apply license from `/Bit/apps osx/Moom`

```shell_session
cd ~/Library/Preferences
rm com.manytricks.Moom.plist
cp /Bit/apps\ osx/Moom/com.manytricks.Moom.plist .
# launch moom
open /Bit/apps\ osx/Moom/Panagiotis\ Sebos.moomlicense
```

* killall cfprefsd

### VMware ###
[up up up](#)

install `brew cask install vmware-fusion`

license found in `/Bit/apps osx/VMWare.Fusion.8/README.txt`

make sure that your virtual machines are on the same version of fusion as the new ones and do a move virtual machines

make sure that all VMs have network settings Autodetect / Bridged or else they may not work.
in this way you change it from vmnet2 to whatever

### Microsoft Remote Desktop ###
[up up up](#)

Download `Microsoft Remote Desktop` from AppStore and If you want to share a configuration you can select the target under My Desktops and either right-click and select Export or from the Microsoft Remote Desktop File menu select Export.

The file that contains the information shown in the Microsoft Remote Desktop window, sans any passwords which are stored in your Keychain, is located at: `/Users/$USER/Library/Containers/com.microsoft.rdc.mac/Data/Library/Preferences/com.microsoft.rdc.mac.plist`

```shell_session
open /Applications/Microsoft\ Remote\ Desktop.app
# quit
rm /Users/$USER/Library/Containers/com.microsoft.rdc.mac/Data/Library/Preferences/com.microsoft.rdc.mac.plist
cp /Bit/apps\ osx/Microsoft\ Remote\ Desktop/com.microsoft.rdc.mac.plist /Users/$USER/Library/Containers/com.microsoft.rdc.mac/Data/Library/Preferences/com.microsoft.rdc.mac.plist
```

* killall cfprefsd

### GoPro ###
[up up up](#)

install GoPro Studio

```shell_session
BD='{{site.backup_location}}'

cp -R ${BD}/Users/psebos/Library/Application\ Support/com.gopro.GoPro-Studio ~/Library/Application\ Support/
cp -R ${BD}/Users/psebos/Library/Application\ Support/com.gopro.GoPro-Studio-GoPro-Importer ~/Library/Application\ Support/
cp -R ${BD}/Users/psebos/Library/Caches/com.gopro.GoPro-Studio ~/Library/Caches/
cp -R ${BD}/Users/psebos/Library/Preferences/com.GoPro* ~/Library/Preferences/
cp -R ${BD}/Users/psebos/Library/Preferences/com.gopro* ~/Library/Preferences/

```

### beets :gun: ###
[up up up](#)

install beets

```shell_session
http://blog.manbolo.com/2013/02/04/how-to-install-python-3-and-pydev-on-osx
brew install python3
pip3 install beets
```

I left this out of reference. The preferred way is via brew

```shell_session
sudo easy_install pip
sudo pip install --proxy 127.0.0.1:3128 --upgrade pip
cd /usr/local/bin
sudo chown psebos:staff pip* # if it is there

sudo chown -R psebos:staff /Users/psebos/Library/Python
sudo chmod -R go+rx /Library/Python/2.7/site-packages


pip install beets
```

### proselint ###
[up up up](#)

```shell_session
pip3 install proselint
```

### rb-appscript ###
[up up up](#)

rb-appscript is a way to right applescript through ruby.

Links are
* [http://appscript.sourceforge.net/rb-appscript/index.html](http://appscript.sourceforge.net/rb-appscript/index.html)
* [https://github.com/mattneub/appscript](https://github.com/mattneub/appscript)

Installing is difficult because there is some type of bug with the tests ([https://github.com/mattneub/appscript/issues/12](https://github.com/mattneub/appscript/issues/12)). In order to make it work do the following:

```shell_session
clone appscript (https://github.com/mattneub/appscript)
git clone https://github.com/mattneub/appscript.git
cd appscript/rb-appscript/trunk
ruby extconf.rb
make
make install
gem build ./rb-appscript.gemspec
gem install ./rb-appscript-0.6.1.gem
```

### update /etc/hosts ####
[up up up](#)

copy it from the backup


### update /etc/pf.bog ####
[up up up](#)

copy it from the backup and follow the instructions in `/etc/pf.bog`


### Spelling ###
[up up up](#)

```shell_session
cd ~/Library/Spelling

ln -s /Dropbox/sync/Library/Spelling/English\ \&\ Greek.aff
ln -s /Dropbox/sync/Library/Spelling/English\ \&\ Greek.dic
System Preferences > Keyboard > Text
Spelling: Set up...
Uncheck everything but the new dictionary
move it on top

Spelling: English & Greek (Library)
```

**Note: 20160504** When using this dictionary it makes the System Preferences > Keyboard > Input Sources (and Language & Region) to crash


### nvalt ###
[up up up](#)

brew cask install nvalt

```shell_session
cd ~/Library/Preferences
rm net.elasticthreads.nv.plist
ln -s /Dropbox/sync/Library/Preferences/net.elasticthreads.nv.plist
killall cfprefsd
```

### Adobe Cloud  ###
[up up up](#)

follow [this thread](https://forums.adobe.com/thread/2218390) and ged rid off any previous installation.

here is what you should do

```shell_session
$ sudo \rm -rf /Applications/Utilities/Adobe\ Installers
# check for adobe stuff in
/Library/LaunchDaemons
/Library/LaunchAgents
/Users/psebos/Library/LaunchAgents

$ sudo rm /Library/Preferences/com.adobe.*
$ rm -rf ~/Library/Preferences/Adobe* ~/Library/Preferences/com.adobe.*
$ rm -rf ~/Library/Application\ Support/Adobe
$ sudo rm -rf /Library/Application\ Support/Adobe* /Library/Application\ Support/regid.1986-12.com.adobe/
```

download the [Creative Cloud Cleaner Tool](https://helpx.adobe.com/gr_en/creative-cloud/kb/cc-cleaner-tool-installation-problems.html) and run it.

Login to Adobe and download the Creative Cloud. Download all the apps you want


### office 2016 ###
[up up up](#)

#### sixty ####

go through this [link](https://www.microsofthup.com/hupemea1/orderdetail.aspx?culture=en-GB&receipt_id=477125300&local=false)

product key is 3N6HP-M49Y3-9YMFX-CYHPB-7MHPW

#### wks-0088-25 ####

install from ISO

### thunderbird migration ###
[up up up](#)

* launch thunderbird on the destination machine
* quit thunderbird and

```shell_session
cd /Users/psebos/Library
\rm -rf Thunderbird/Profiles/*
\rm -rf Thunderbird/profiles.ini
\rm -rf  ./Thunderbird/Crash\ Reports
\rm -rf ./Caches/Thunderbird
cp /Bit/apps\ osx/Thunderbird/Library/Preferences/org.mozilla.thunderbird.plist ./Preferences/
cp /Bit/apps\ osx/Thunderbird/Library/Thunderbird/profiles.ini ./Thunderbird/
```

make sure that you copied `/data/mail` from the old server

launch thunderbird and you should be golden

### Hacks ###
[up up up](#)

#### How to burn a bootable iso to a usb stick ####

* `hdiutil convert -format UDRW -o debian-live-8.3.0-amd64-standard.img debian-live-8.3.0-amd64-standard.iso`
* `mv debian-live-8.3.0-amd64-standard.img.dmg debian-live-8.3.0-amd64-standard.img`
* `diskutil list`

```shell_session
/dev/disk3 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *8.0 GB     disk3
   1:                 DOS_FAT_32 TEST                    8.0 GB     disk3s1
```

* `iskdiskutil unmountDisk /dev/dN`
* `sudo dd if=./debian-live-8.3.0-amd64-standard.img of=/dev/rdiskN bs=1M`
* `diskutil eject /dev/diskN`

reference for this is [here](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-mac-osx)

#### How to install a kext ####

somehow install the kext

```shell_session
touch /System/Library/Extensions
kextcache -u /
kextcache -system-prelinked-kernel
kextcache -system-caches
sudo diskutil repairPermissions / # this is not valid for >= 10.11
```

#### How to unpack and pack pkg file ####

uncompress Payload

```shell_session
mkdir Foo
cd Foo
xar -xf ../Foo.pkg
cd foo.pkg
cat Payload | gunzip -dc |cpio -i
# edit Foo.app/*
rm Payload
find ./Foo.app | cpio -o | gzip -c > Payload
mkbom Foo.app Bom # or edit Bom
# edit PackageInfo
rm -rf Foo.app
cd ..
xar -cf ../Foo-new.pkg
```

uncompress Bom

```shell_session
lsbom Bom
```


reference [link](https://coolaj86.com/articles/how-to-unpackage-and-repackage-pkg-osx.html) and [link](http://stackoverflow.com/questions/11298855/how-to-unpack-and-pack-pkg-file)

#### How to uninstall office ####

remove
`/Applications/Microsoft Office 2011`
`~/Library/Preferences/com.microsoft*`
`~/Library/Preferences/ByHost/com.microsoft*`
`~/Library/Application Support/Microsoft/Office*`
`/Library/LaunchDaemons/com.microsoft.office.licensing.helper.plist`
`/Library/Preferences/com.microsoft.office.licensing.plist`
`/Library/PrivilegedHelperTools/com.microsoft.office.licensing.helper`
`/Library/Application Support/Microsoft`
`/Library/Fonts/Microsoft`
`/Library/Receipts/Office2011_*`
`~/Documents/Microsoft User Data`

empty trash and restart

reference for office 2011 [link](https://support.office.com/en-us/article/Troubleshoot-Office-2011-for-Mac-issues-by-completely-uninstalling-before-you-reinstall-ba8d8d13-0015-4eea-b60b-7719c2cedd17)

reference for office 2016 [link](https://support.office.com/en-us/article/Uninstall-Office-2016-for-Mac-eefa1199-5b58-43af-8a3d-b73dc1a8cae3)

```shell_session
lakiss-mac:Sketch 3.4.3 [kg] lakis$ ./sketch_v3
2015-12-02 22:29:28.105 sketch_v3[325:507] --- Sketch v3 Keyfilemaker by Team NOY ---
2015-12-02 22:29:28.108 sketch_v3[325:507] Creating license file: /Users/lakis/Library/Application Support/com.bohemiancoding.sketch3/.license
lakiss-mac:Sketch 3.4.3 [kg] lakis$ !ca
-bash: !ca: event not found
lakiss-mac:Sketch 3.4.3 [kg] lakis$ more "/Users/lakis/Library/Application Support/com.bohemiancoding.sketch3/.license"
{
  "payload" : {
    "status" : "",
    "lid" : "",
    "application" : "sketch3",
    "type" : "static",
    "udid" : "",
    "expiration" : 2147483647
  },
  "meta" : {
    "sign" : ""
  }
}
lakiss-mac:Sketch 3.4.3 [kg] lakis$
```

### 1Password Browser Plugin Problem ###
[up up up](#)

browser plugins do not work in bog. See [this](https://support.1password.com/configure-os-x-proxy/) post.

Goto Proxy setting and exlude **both** 127.0.0.1 and localhost

### El Capitan DNS resolver ###
[up up up](#)

**This probably is not required for >=10.12**

The solution in this problem is being outlined [here](http://slaptijack.com/system-administration/goodbye-discoveryd-hello-again-mdnsresponder/)

I did these steps

`$ sudo vi /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist`

```xml
        <key>ProgramArguments</key>
        <array>
                <string>/usr/sbin/mDNSResponder</string>
                <string>-AlwaysAppendSearchDomains</string>
        </array>
```

and finally
`$ sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist`
`$ sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist`


now when I resolve something:

```shell_session
$ ping wks-0088-26
PING wks-0088-26.bankofgreece.gr (10.130.22.118): 56 data bytes
64 bytes from 10.130.22.118: icmp_seq=0 ttl=128 time=0.329 ms
```

and as always man is your friend
`$ man mDNSResponder`

**Sources**

```shell_session
http://apple.stackexchange.com/questions/24018/dns-lookups-fail-with-e-g-ping-but-work-with-host
http://osxdaily.com/2015/11/16/howto-flush-dns-cache-os-x-elcap/
http://slaptijack.com/system-administration/goodbye-discoveryd-hello-again-mdnsresponder/
https://coderwall.com/p/fzcyha/learn-about-alwaysappendsearchdomains-in-osx-mountain-lion
http://serverfault.com/questions/583844/osx-nslookup-can-resolve-a-lan-hostname-but-ping-cannot
```

### NTP server configuration ###
[up up up](#)

In order to setup ntp (e.g. BoG computer) you can use the following commands:

```shell_session
Viewing or Changing Network Time Server Usage

To see if a network time server is being used:
$ sudo systemsetup -getusingnetworktime

To enable or disable use of a network time server:
$ sudo systemsetup -setusingnetworktime (on|off)

To view the current network time server:
$ sudo systemsetup -getnetworktimeserver

To specify a network time server:
$ sudo systemsetup -setnetworktimeserver timeserver
```

In my case I did:

```shell_session
$ sudo systemsetup -setnetworktimeserver ntp1.bankofgreece.gr
$ sudo systemsetup -setusingnetworktime on
$ sudo systemsetup -getusingnetworktime
```

### Keyboard > Input Sources crashing ###
[up up up](#)

Since I have installed my spelling dictionary in `~psebos/Library/Spelling` I faced the problem that the `Keyboard > Input Sources` preference pane kept on crashing on me.

In order to debug it I created a different user `jo` and tried to monitor `~jo/Library` for changes. It turned out that when I copied the files and enabled the speller through `Keyboard > Text` the `Keyboard > Input Sources` started crashing.

The problem was `~psebos/Library/.GlobalPreferences.plist`. When selecting the speller (which was a combined Greek and English together) the file changed and added another language under `AppleLanguage`:

```xml
        <key>AppleLanguages</key>
        <array>
                <string>en</string>
                <string>el</string>
                <string>english &amp; Greek</string>
        </array>
```

This caused the `Keyboard > Input Sources` to crash. I fixed by removing the one language:

```xml
        <key>AppleLanguages</key>
        <array>
                <string>en</string>
                <string>el</string>
        </array>
```

There are not really any references. However [this](https://discussions.apple.com/thread/7023625?start=0&tstart=0) and [this](http://apple.stackexchange.com/questions/53171/how-to-set-input-sources-for-all-users-at-the-same-time) source helped me.

### Jumping menus ###
[up up up](#)

So after a while, I figured out what was going wrong with the jumping menus. It seemed as if an app was trying to run on the menu bar and exited immediately. No trace in the logs no anything. Eventually after reading [this post](https://discussions.apple.com/thread/7411259?start=0&tstart=0) it turned out that I had to turn off `Settings > Displays > Show mirroring options in the menu bar when available`

### Hide TimeMachine's Folder icon ###
[up up up](#)

according to [this](https://discussions.apple.com/thread/7264341?start=0&tstart=0)

```shell_session
chflags hidden "/Volumes/timemachine"
killall Finder

To unhide it:
chflags nohidden "/Volumes/timemachine"
killall Finder
```

### Mount EFI ###
[up up up](#)

```shell_session
$ diskutil list

/dev/disk1 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *256.1 GB   disk1
   1:                        EFI EFI                     209.7 MB   disk1s1
   2:                  Apple_HFS Macintosh HD            255.2 GB   disk1s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk1s3


mkdir /mnt/EFI (and change the permissions owned by psebos)

sudo mount -t msdos /dev/disk1s1 /mnt/EFI

sudo umount /mnt/EFI
```

### Install nokogiri ###
[up up up](#)

`Installing nokogiri 1.6.8.1 with native extensions` fails .

The solution is to `brew uninstall xz`

source: https://github.com/sparklemotion/nokogiri/issues/1483

### Send mails from the command line ###
[up up up](#)

use `msmtp` and `brew isntall msmtp`

create `~/.msmtprc`

```shell_session
defaults
logfile ~/.msmtp.log

account        gmail
host           smtp.gmail.com
port           587
protocol       smtp
auth           on
from           Your Friendly Reminder
user           panagiotis.sebos@gmail.com
tls            on
# Enable or disable checks of the server certificate.  WARNING:  When  the  checks  are
# disabled, TLS sessions will be vulnerable to man-in-the-middle attacks!
#tls_trust_file /usr/local/opt/curl-ca-bundle/share/ca-bundle.crt
tls_certcheck off

# socks proxy from wks-0088-25
#proxy_host      127.0.0.1
#proxy_port      7070

# Set a default account
# You need to set a default account for Mail
account default : gmail
```

create '~/.mailrc'

```shell_session
set sendmail="/usr/local/bin/msmtp"
```

* goto [here](https://security.google.com/settings/security/apppasswords) and create a new app password.
* launch `Keychain Access`
* File > New Password Item
* keychain item name: smtp://smtp.gmail.com
* account name: panagiotis.sebos@gmail.com
* password: app password

now you can send an email by using:

```shell_session
echo "Hello world" | mail -s "msmtp test at `date`" psebos@gmail.com
```

source: http://apple.stackexchange.com/questions/12387/how-to-send-an-email-from-command-line
