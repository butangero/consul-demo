# -*- mode: ruby -*-
# vi: set ft=ruby :
$script = <<SCRIPT

echo Installing dependencies...
sudo apt-get update
sudo apt-get install -y unzip curl tmux screen

echo Fetching Consul...
cd /tmp/
curl https://releases.hashicorp.com/consul/0.6.4/consul_0.6.4_linux_amd64.zip -o consul.zip

echo Installing Consul...
unzip consul.zip
sudo chmod +x consul
sudo mv consul /usr/bin/consul

sudo mkdir /etc/consul.d
sudo chmod a+w /etc/consul.d
ln -s /vagrant/files/bash_aliases /home/vagrant/.bash_aliases

#### travis stuff ######
local_ip=`/sbin/ifconfig  | grep 'inet addr:'| grep '172.20.20' | cut -d: -f2 | awk '{ print $1}'`
consul agent  -atlas-join -atlas=butangero/infrastructure -atlas-token="VbigYxjTwWCCAA.atlasv1.4wj6dgZt1HMjc4BPLd2EsOB5VELYnJcaOCiSUIANnWVX2m1NyrHFzyWmjEyqnh6e4Ic" -server -bootstrap-expect 3     -data-dir /tmp/consul -node=$HOSTNAME -bind=$local_ip -config-dir=/etc/consul.d


SCRIPT

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "debian/wheezy64"

  config.vm.provision "shell", inline: $script

  config.vm.define "n1" do |n1|
      n1.vm.hostname = "n1"
      n1.vm.network "private_network", ip: "172.20.20.10"
  end

  config.vm.define "n2" do |n2|
      n2.vm.hostname = "n2"
      n2.vm.network "private_network", ip: "172.20.20.11"
  end

  config.vm.define "n3" do |n2|
      n2.vm.hostname = "n3"
      n2.vm.network "private_network", ip: "172.20.20.12"
  end
end
