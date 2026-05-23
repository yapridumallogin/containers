Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/noble64"

    config.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.cpus = 1
    end
  
    config.vm.network "forwarded_port", guest: 80, host: 8080
    config.vm.network "private_network", type: "dhcp"
  
    config.vm.provision "file", source: "userdata", destination: "/tmp/userdata"
    config.vm.provision "file", source: "meta-data", destination: "/tmp/meta-data"

    config.vm.provision "shell", inline: <<-SHELL
        sudo cloud-init init --local
        sudo cp /tmp/userdata /var/lib/cloud/instances/vagrant-web-server/user-data.txt
        sudo cp /tmp/meta-data /var/lib/cloud/instances/vagrant-web-server/meta-data.json
        sudo cloud-init init
        sudo cloud-init modules --mode=config
        sudo cloud-init modules --mode=final
    SHELL
end