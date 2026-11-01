Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.define "ctrl01" do |m|
    m.vm.hostname = "ctrl01"
    m.vm.network "private_network",
      ip: "10.10.99.10",
      netmask: "255.255.255.0",
      virtualbox__intnet: "intnet-mgmt"
    m.vm.provision "shell", run: "always",
      inline: "ip route replace 10.10.10.0/24 via 10.10.99.1"

    m.vm.provider "virtualbox" do |vb|
      vb.name   = "ctrl01"
      vb.memory = 1024
      vb.cpus   = 1
    end
  end
  config.vm.define "app01" do |m|
  m.vm.hostname = "app01"
  m.vm.network "private_network",
    ip: "10.10.10.12",
    netmask: "255.255.255.0",
    virtualbox__intnet: "intnet-servers"
  m.vm.provision "shell", run: "always",
    inline: "ip route replace 10.10.99.0/24 via 10.10.10.1"
  m.vm.provider "virtualbox" do |vb|
    vb.name   = "app01"
    vb.memory = 2048
    vb.cpus   = 1
  end
end
end