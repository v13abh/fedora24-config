# Fedora 24 Configuration Guide for DevOps 

## Recommended Installs

### Install VirtualBox

First, add the [VirtualBox](https://www.virtualbox.org/wiki/Documentation) repo and then update the system:

	cd /etc/yum.repos.d/
	sudo wget http://download.virtualbox.org/virtualbox/rpm/fedora/virtualbox.repo
	sudo dnf update

We need to ensure that we are running the latest install kernel version. The version numbers from each of these outputs should match:

	rpm -qa kernel |sort -V |tail -n 1
	uname -r

If they do not match, reboot the computer and check again.

Next we need to install VirtualBox dependencies:

	dnf install binutils gcc make patch libgomp glibc-headers glibc-devel kernel-headers kernel-devel dkms
	
And then install VirtualBox:

	dnf install VirtualBox-5.0

*NOTE: It's possible that the previous command completed with errors. This happens when we have Secure Boot enabled with SELinux as Oracle can't sign kernel modules using the Fedora key.  Disable Secure Boot and then run the following command to build the needed kernel modules:*

	/usr/lib/virtualbox/vboxdrv.sh setup

Finally, we need to add our username to the `vboxuers` group:

	usermod -a -G vboxusers <username>

## Install Vagrant

Install [Vagrant](https://www.vagrantup.com/) with the following command:

	dnf install vagrant

Fedora uses `libvirt` as a default Vagrant provider so in order to use Virtualbox we need to set an environment variable:

	echo "export VAGRANT_DEFAULT_PROVIDER=virtualbox" >> ~/.bashrc

Next, we will install a few required packages that Fedora 24 needs:

	dnf install ruby-devel redhat-rpm-config zlib-devel

Finally, we will install a couple of handy plugins to make using Vagrant easier:

	vagrant plugin install vagrant-cachier
	vagrant plugin install vagrant-hostmanager

### Install Ansible

Install [Ansible](https://www.ansible.com/) using the following command:

	dnf install ansible

## Optional Installs

These are not required but may be selected based on user preference.

### Install VIM Enhanced

We can install [VIM Enhanced](https://apps.fedoraproject.org/packages/vim) with the following command:

	dnf install vim-enhanced

### Install Gnome Tweak Tool

Fedora 24 uses the gnome-shell desktop environment. To change some of gnome-shell's settings, you can install the [Gnome Tweak Tool](https://wiki.gnome.org/Apps/GnomeTweakTool): 	

	sudo dnf install gnome-tweak-tool

### Install Gnome Shell Extensions

There is a section in Gnome Tweak Tool to modify the default built in extensions for Gnome Shell. We can find more extensions on the [Gnome Shell Extensions](https://extensions.gnome.org/about/) website:

	https://extensions.gnome.org

### Activate RPMFusion Repository

The [RPMFusion Repository](http://rpmfusion.org/) provides some free and non-free software for Fedora. The repo can be used via the command line. The repository is meant to provide stable and tested packages for Fedora so it is highly recommended to activate it on your system with:

	sudo rpm -ivh http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-stable.noarch.rpm


### Install VLC Media Player

[VLC Media Player](http://www.videolan.org/vlc/index.html) is a superior video player that supports neraly every video format available and is included on the RPMFusion repo that we just activated:

	sudo dnf install vlc

### Install Google Chrome

It's always nice to have a second browser and [Google Chrome](https://www.google.com/chrome/browser/desktop/) allows us to easily sync all of our bookmarks across all of our devices. First we will need to create the repo file:

	sudo cat << EOF > /etc/yum.repos.d/google-chrome.repo
	[google-chrome]
	name=google-chrome - \$basearch
	baseurl=http://dl.google.com/linux/chrome/rpm/stable/\$basearch
	enabled=1
	gpgcheck=1
	gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub
	EOF

Then we can install the stable version of Chrome:

	sudo dnf install google-chrome-stable

(We can also install other Google products from this repo such as Google Earth, Music Manager, etc.)\

### Install Hipchat 

We like to use [HipChat](https://www.hipchat.com/) for Team Communication and Collaboration. First we will need to create the repo file:

	sudo cat << EOF > /etc/yum.repos.d/hipchat.repo
	[atlassian-hipchat]
	name=Atlassian Hipchat
	baseurl=https://atlassian.artifactoryonline.com/atlassian/hipchat-yum-client/
	enabled=1
	gpgcheck=0
	EOF

(Yes, Atlassian does not appear to be currently signing the repo...)

Now we can install hipchat:

	sudo dnf install hipchat4

### Install Cheat

[Cheat](https://github.com/chrisallenlane/cheat) allows us to creat and view "cheatsheets" on the command line so we can remember commands that we use frequently but not often enough to remember:

	pip install cheat

And we can enable syntax highlighting:

	export CHEATCOLORS=true

Newly created or modified cheat files are stored in `~/.cheat/`.

### Install Liquid Prompt

[Liquid Prompt](https://github.com/nojhan/liquidprompt) gives us a dynamic prompt that integrates nicely with git and reminds us of changes to files and commits waiting to be pushed. We install by cloning from Github and adding to .bashrc:

	cd
	git clone https://github.com/nojhan/liquidprompt.git .liquidprompt
	source .liquidprompt/liquidprompt

And then we add the following to .bashrc:

	# Only load Liquid Prompt in interactive shells, not from a script or from scp
	[[ $- = *i* ]] && source ~/.liquidprompt/liquidprompt


