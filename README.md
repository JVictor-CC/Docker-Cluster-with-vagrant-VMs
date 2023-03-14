# Docker-Cluster-with-vagrant-VMs

Defining a docker swarm cluster with vagrant

<details><summary><h3>Install vagrant on ubuntu/debian</h3></summary>

```
$ wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
$ echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
$ sudo apt update && sudo apt install vagrant
```
</details>

Then using visual studio code, I initialize vagrant with `vagrant init`

### Configuring Vagrantfile

```
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

```

I've configured 3 machines with 1024m of memory and 1 cpu each. all running ubuntu server 22.04

### Vagrant Commands

Start the virtual machines:      `vagrant up`

Remote access to a machine:   `vagrant ssh <node-name>`

Destroy the virtual machines:    `vagrant destroy -f`

### Conclusion

with all done, we can access the master node `vagrant ssh node-1`

as root `sudo su`, we check if everything runs ok `docker node ls`

the output should be like this:
```
root@node-1:/# docker node ls
ID                            HOSTNAME   STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
mmbkvhupt15a0l778vsactgqq *   node-1     Ready     Active         Leader           23.0.1
wux8rzbmkcbwvuipe56e6vc4q     node-2     Ready     Active                          23.0.1
p421x5tawl8qfi5pknffxxy33     node-3     Ready     Active                          23.0.1
```


