# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {

  :SQLsrv => {
    :box_name => "ubuntu/bionic64",
    :net => [
               {ip: '192.168.6.2', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "net"},

            ]
  }, 
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s

        config.vm.provider "virtualbox" do |v|
          v.memory = 1024
        end

        boxconfig[:net].each do |ipconf|
          box.vm.network "private_network", ipconf
        end

        if boxconfig.key?(:public)
          box.vm.network "public_network", boxconfig[:public]
        end

        box.vm.provision "shell", inline: <<-SHELL
          mkdir -p ~root/.ssh
                cp ~vagrant/.ssh/auth* ~root/.ssh
		echo "deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main" | sudo tee /etc/apt/sources.list.d/pgdg.list
		wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
		apt-get update
		DEBIAN_FRONTEND=noninteractive apt-get install -y postgresql-8.4
	SHELL

      end

  end


end
