$script1 = <<SCRIPT

echo Installing depedencies...
sudo apt-get install -y unzip

echo Fetching Serf...
cd /tmp/
wget https://dl.bintray.com/mitchellh/serf/0.5.0_linux_amd64.zip -O serf.zip

echo Installing Serf...
unzip serf.zip
sudo chmod +x serf
sudo mv serf /usr/bin/serf

SCRIPT

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.provision "shell", inline: $script1
  config.vm.network "private_network", ip: "172.20.20.10"

  config.vm.provision :docker do |d|
    d.pull_images "ubuntu"
  end
end
