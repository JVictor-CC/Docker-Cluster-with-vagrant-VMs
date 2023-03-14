Vagrant.configure("2") do |config|
  (1..3).each do |i|
    config.vm.define "node-#{i}" do |node|
      node.vm.box = "bento/ubuntu-22.04"
      node.vm.hostname = "node-#{i}"
      node.vm.network "private_network", ip: "10.10.10.10#{i}"
      node.vm.provider "virtualbox" do |vb|
        vb.name = "node-#{i}"
        vb.memory = "1024"
        vb.cpus = "1"
      end
      node.vm.provision "shell", path: "install-docker.sh"
      if "node-#{i}" == "node-1"
        node.vm.provision "shell", path: "master.sh"
      else
        node.vm.provision "shell", path: "worker.sh"
      end
    end
  end
end
