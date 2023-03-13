# Docker-Cluster-with-vagrant-VMs

<details><summary><h3>Install vagrant on ubuntu/debian</h3></summary>

```
$ wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
$ echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
$ sudo apt update && sudo apt install vagrant
```
</details>

Then using visual studio code, i initialize vagrant with `vagrant init`

### Configuring Vagrantfile

```
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"      # specify the distro you are going to use.

end
```

### Vagrant Commands

Start the virtual machine: `vagrant up`
Remote access to the machine: `vagrant ssh`
Destroy the virtual machine: `vagrant destroy -f`

