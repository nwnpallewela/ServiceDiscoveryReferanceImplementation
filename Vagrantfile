# -*- mode: ruby -*-
# vi: set ft=ruby :
$script = <<SCRIPT

echo Installing dependencies...
sudo apt-get update
sudo apt-get install -y unzip curl
sudo apt-get install -y apt-transport-https
SCRIPT

$script1 = <<SCRIPT

echo Consul server...
docker run -d -h node1 -v /mnt:/data \
    -p 172.20.20.10:8300:8300 \
    -p 172.20.20.10:8301:8301 \
    -p 172.20.20.10:8301:8301/udp \
    -p 172.20.20.10:8302:8302 \
    -p 172.20.20.10:8302:8302/udp \
    -p 172.20.20.10:8400:8400 \
    -p 172.20.20.10:8500:8500 \
    -p 172.17.0.1:53:53/udp \
    progrium/consul -server -advertise 172.20.20.10 -bootstrap-expect 2

docker run -i -d \
-v /var/run/docker.sock:/tmp/docker.sock \
-h 172.20.20.10 gliderlabs/registrator \
consul://172.20.20.10:8500

docker run -i -d \
-e "SERVICE_NAME=simple" \
-p 172.20.20.10:8000:80 nuwanpallewela/pythonserver

docker run -i -d \
-e "CONSUL=172.20.20.10:8500" \
-e "SERVICE=simple" \
-p 172.20.20.10:80:80 nuwanpallewela/drcon
SCRIPT

$script2 = <<SCRIPT

echo Consul agent...
docker run -d -h node2 -v /mnt:/data  \
    -p 172.20.20.11:8300:8300 \
    -p 172.20.20.11:8301:8301 \
    -p 172.20.20.11:8301:8301/udp \
    -p 172.20.20.11:8302:8302 \
    -p 172.20.20.11:8302:8302/udp \
    -p 172.20.20.11:8400:8400 \
    -p 172.20.20.11:8500:8500 \
    -p 172.17.0.1:53:53/udp \
    progrium/consul -server -advertise 172.20.20.11 -join 172.20.20.10

docker run -i -d \
-v /var/run/docker.sock:/tmp/docker.sock \
-h 172.20.20.11 gliderlabs/registrator \
consul://172.20.20.11:8500

docker run -i -d \
-e "SERVICE_NAME=simple" \
-p 172.20.20.11:8000:80 nuwanpallewela/pythonserver
SCRIPT

# Specify a custom Vagrant box for the demo
DEMO_BOX_NAME =  ENV['DEMO_BOX_NAME'] || "debian/jessie64"

# Vagrantfile API/syntax version.
# NB: Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = DEMO_BOX_NAME

 config.vm.provision "shell", inline: $script
 config.vm.provision "docker" do |d|
  end
  config.vm.define "n1" do |n1|
      n1.vm.hostname = "n1"
      n1.vm.network "private_network", ip: "172.20.20.10"
n1.vm.provision "shell", inline: $script1
  end
  config.vm.define "n2" do |n2|
      n2.vm.hostname = "n2"
      n2.vm.network "private_network", ip: "172.20.20.11"
n2.vm.provision "shell", inline: $script2
  end
end

