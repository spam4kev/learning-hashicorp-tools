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
```
curl https://releases.hashicorp.com/vagrant/2.0.3/vagrant_2.0.3_x86_64.rpm -O
curl https://download.virtualbox.org/virtualbox/5.2.8/VirtualBox-5.2-5.2.8_121009_fedora26-1.x86_64.rpm -O
curl http://download.virtualbox.org/virtualbox/5.2.8/VBoxGuestAdditions_5.2.8.iso -O
curl https://releases.hashicorp.com/packer/1.2.2/packer_1.2.2_linux_amd64.zip -O
curl http://ftp.osuosl.org/pub/centos/7/isos/x86_64/sha256sum.txt.asc -O
curl http://linux.mirrors.es.net/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1708.iso -O
```
# Install deps
```
sudo dnf install ./vagrant_2.0.3_x86_64.rpm ./VirtualBox-5.2-5.2.8_121009_fedora26-1.x86_64.rpm -y
sudo dnf install kernel-devel kernel-headers dkms ## https://www.if-not-true-then-false.com/2010/install-virtualbox-guest-additions-on-fedora-centos-red-hat-rhel/
sudo dnf install gcc kernel-devel kernel-headers dkms make bzip2 perl
KERN_DIR=/usr/src/kernels/`uname -r`/build
sudo n -s /data/code/github/learning-hashitools/packer /usr/sbin/
packer --version
1.1.0
```
# Start Stuff
## Create a base box (https://www.vagrantup.com/docs/virtualbox/boxes.html)
```
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
##https://www.packer.io/docs/builders/virtualbox-iso.html
[[ $shadl -eq $shagold ]] &&  sed -i "s/\"iso_checksum\": .*/\"iso_checksum\": \"$shagold\",/" centos-vagrant.json  || echo "iso sha doesnt match - DO NOT CONTINUE"
```

```
$ mkdir http && touch http/ks.cfg

```
* example of ks.cfg:
** http://softwaretester.info/create-simple-centos-7-virtualbox-with-packer/
** https://github.com/geerlingguy/packer-centos-7/tree/master/http
* reference manual for ks.cfg: ** https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/installation_guide/sect-kickstart-syntax

# Run the dang thang

```
$ packer validate packer-centos7.json
Template validated successfully.
$ packer build packer-centos7.json
virtualbox-iso output will be in this color.

==> virtualbox-iso: Downloading or copying Guest additions
    virtualbox-iso: Downloading or copying: file:///usr/share/virtualbox/VBoxGuestAdditions.iso
==> virtualbox-iso: Downloading or copying ISO
    virtualbox-iso: Downloading or copying: file:///data/code/github/learning-hashicorp-tools/CentOS-7-x86_64-Minimal-1708.iso
==> virtualbox-iso: Starting HTTP server on port 8724
==> virtualbox-iso: Creating virtual machine...
==> virtualbox-iso: Creating hard drive...
==> virtualbox-iso: Creating forwarded port mapping for communicator (SSH, WinRM, etc) (host port 2361)
==> virtualbox-iso: Executing custom VBoxManage commands...
    virtualbox-iso: Executing: modifyvm ORG_CENTOS7 --memory 2048
    virtualbox-iso: Executing: modifyvm ORG_CENTOS7 --cpus 2
    virtualbox-iso: Executing: modifyvm ORG_CENTOS7 --audio none
==> virtualbox-iso: Starting the virtual machine...
==> virtualbox-iso: Waiting 10s for boot...
==> virtualbox-iso: Typing the boot command...
==> virtualbox-iso: Waiting for SSH to become available...
vagrant

==> virtualbox-iso: Connected to SSH!
==> virtualbox-iso: Uploading VirtualBox version info (5.2.8)
==> virtualbox-iso: Uploading VirtualBox guest additions ISO...
==> virtualbox-iso: Provisioning with shell script: /tmp/packer-shell219150283
    virtualbox-iso: testing123
==> virtualbox-iso: Gracefully halting virtual machine...
==> virtualbox-iso: Preparing to export machine...
    virtualbox-iso: Deleting forwarded port mapping for the communicator (SSH, WinRM, etc) (host port 2361)
==> virtualbox-iso: Exporting virtual machine...
    virtualbox-iso: Executing: export ORG_CENTOS7 --output output-virtualbox-iso/ORG_CENTOS7.ovf
==> virtualbox-iso: Unregistering and deleting virtual machine...
Build 'virtualbox-iso' finished.

==> Builds finished. The artifacts of successful builds are:
--> virtualbox-iso: VM files in directory: output-virtualbox-iso

```



vagrant init
