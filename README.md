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
curl https://releases.hashicorp.com/packer/1.2.2/packer_1.2.2_linux_amd64.zip -O
curl http://ftp.osuosl.org/pub/centos/7/isos/x86_64/sha256sum.txt.asc -O
curl http://linux.mirrors.es.net/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1708.iso -O
# Install deps
sudo dnf install ./vagrant_2.0.3_x86_64.rpm ./VirtualBox-5.2-5.2.8_121009_fedora26-1.x86_64.rpm -y
sudo dnf install kernel-devel kernel-headers dkms ## https://www.if-not-true-then-false.com/2010/install-virtualbox-guest-additions-on-fedora-centos-red-hat-rhel/
sudo dnf install gcc kernel-devel kernel-headers dkms make bzip2 perl
KERN_DIR=/usr/src/kernels/`uname -r`/build
sudo n -s /data/code/github/learning-hashitools/packer /usr/sbin/
packer --version
1.1.0
# Start Stuff
## Create a base box (https://www.vagrantup.com/docs/virtualbox/boxes.html)
## 
KERN_DIR=/usr/src/kernels/`uname -r`/build
mkdir VBoxGuestAdditions
sudo mount -o loop,ro VBoxGuestAdditions_5.2.8.iso VBoxGuestAdditions
sudo su -
cd data/code/github/learning-hashitools/
export KERN_VER=4.15.14-300.fc27.x86_64
sh VBoxGuestAdditions/VBoxLinuxAdditions.run
exit
sudo umount VBoxGuestAdditions/
shagold=$(grep "$(ls *Minimal*)" sha256sum.txt.asc | cut -f1 -d' ') #https://wiki.centos.org/TipsAndTricks/sha256sum
shadl=$(sha256sum $(ls *Minimal*) | cut -f1 -d' ')
## https://www.packer.io/docs/builders/virtualbox-iso.html
[[ $shadl -eq $shagold ]] &&  sed -i "s/\"iso_checksum\": .*/\"iso_checksum\": \"$shagold\",/" centos-vagrant.json  || echo "iso sha doesnt match - DO NOT CONTINUE"






vagrant init

