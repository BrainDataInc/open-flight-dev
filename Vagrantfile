# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
export DEBIAN_FRONTEND=noninteractive

sudo apt-get update
sudo apt-get install -y --force-yes \
     git-core python-dev python-pip

git clone https://github.com/openstack-dev/devstack.git

cat > devstack/local.conf << EOF
HOST_IP=192.168.33.10
DATABASE_PASSWORD=password
RABBIT_PASSWORD=password
SERVICE_TOKEN=password
SERVICE_PASSWORD=password
ADMIN_PASSWORD=password
PUBLIC_INTERFACE=eth0
LOGFILE=/home/stack/stack.sh.log
disable_service n-net
enable_service q-svc
enable_service q-agt
enable_service q-dhcp
enable_service q-l3
enable_service q-meta
enable_service neutron
# Optional, to enable tempest configuration as part of devstack
enable_service tempest
EOF

cp devstack/samples/local.sh devstack/
sudo mkdir -p /opt/stack
sudo chown -R vagrant:vagrant /home/vagrant/devstack
sudo chown -R vagrant:vagrant /opt/stack
SCRIPT

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "openstack"
  config.vm.network "forwarded_port", guest: 5000, host: 5000
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
  end
  config.vm.provision "shell", inline: $script
end
