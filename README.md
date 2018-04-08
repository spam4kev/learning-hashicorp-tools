# Get repo
cd /data/code/github.com/
git clone git@github.com:spam4kev/learning-hashitools.git
cd learning-hashitools
# Get Deps
curl https://releases.hashicorp.com/vagrant/2.0.3/vagrant_2.0.3_x86_64.rpm -O
curl https://download.virtualbox.org/virtualbox/5.2.8/VirtualBox-5.2-5.2.8_121009_fedora26-1.x86_64.rpm -O
scp root@10.11.11.14:/c/media/BitTorrent/operating_systems/centos/CentOS-7-x86_64-Minimal-1503-01.iso /data/
# Install deps
sudo dnf install ./vagrant_2.0.3_x86_64.rpm ./VirtualBox-5.2-5.2.8_121009_fedora26-1.x86_64.rpm -y
