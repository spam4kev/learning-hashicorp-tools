# Build Env
Used an HP Folio13 Laptop
  i5 cpu
  4GB mem
  13G avail storage
Fedore 27
  kernel 4.15.14-300
# Get repo
cd /data/code/github.com/
git clone git@github.com:spam4kev/learning-hashitools.git
cd learning-hashitools
# Get Deps
curl https://releases.hashicorp.com/vagrant/2.0.3/vagrant_2.0.3_x86_64.rpm -O
curl https://download.virtualbox.org/virtualbox/5.2.8/VirtualBox-5.2-5.2.8_121009_fedora26-1.x86_64.rpm -O
curl http://download.virtualbox.org/virtualbox/5.2.8/VBoxGuestAdditions_5.2.8.iso -O
scp root@10.11.11.14:/c/media/BitTorrent/operating_systems/centos/CentOS-7-x86_64-Minimal-1503-01.iso /data/
# Install deps
sudo dnf install ./vagrant_2.0.3_x86_64.rpm ./VirtualBox-5.2-5.2.8_121009_fedora26-1.x86_64.rpm -y
sudo dnf install kernel-devel kernel-headers dkms
## https://www.if-not-true-then-false.com/2010/install-virtualbox-guest-additions-on-fedora-centos-red-hat-rhel/
sudo dnf install gcc kernel-devel kernel-headers dkms make bzip2 perl
KERN_DIR=/usr/src/kernels/`uname -r`/build

# Start Stuff
## Create a base box (https://www.vagrantup.com/docs/virtualbox/boxes.html)
mkdir VBoxGuestAdditions
sudo mount -o loop,ro VBoxGuestAdditions_5.2.8.iso VBoxGuestAdditions
sudo su -
cd data/code/github/learning-hashitools/
sh VBoxGuestAdditions/VBoxLinuxAdditions.run

vagrant init

