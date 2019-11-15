# -*- mode: ruby -*-
# vim: set ft=ruby :
# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :iR => {
  :box_name => "centos/7",
  :net => [
          {adapter: 2, auto_config: false, virtualbox__intnet: "bonding"},
          {adapter: 3, auto_config: false, virtualbox__intnet: "bonding"},
          {adapter: 4, auto_config: false, virtualbox__intnet: "teaming"},
          {adapter: 5, auto_config: false, virtualbox__intnet: "teaming"},
          ] },
  :cR => {
  :box_name => "centos/7",
  :net => [
          {adapter: 2, auto_config: false, virtualbox__intnet: "bonding"},
          {adapter: 3, auto_config: false, virtualbox__intnet: "bonding"},
          {adapter: 4, auto_config: false, virtualbox__intnet: "teaming"},
          {adapter: 5, auto_config: false, virtualbox__intnet: "teaming"},
          {adapter: 6, auto_config: false, virtualbox__intnet: "vlans"},
          ] },
  :tC1 => {
  :box_name => "centos/7",
  :net => [  
          {adapter: 2, auto_config: false, virtualbox__intnet: "vlans"},
  ] },
  :tS1 => {
  :box_name => "centos/7",
  :net => [
          {adapter: 2, auto_config: false, virtualbox__intnet: "vlans"},
  ] },
  :tC2 => {
  :box_name => "centos/7",
  :net => [
          {adapter: 2, auto_config: false, virtualbox__intnet: "vlans"},
  ] },
  :tS2 => {
  :box_name => "centos/7",
  :net => [
          {adapter: 2, auto_config: false, virtualbox__intnet: "vlans"},
  ] } }

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|
    box.vm.box = boxconfig[:box_name]
    box.vm.host_name = boxname.to_s
    boxconfig[:net].each do |ipconf|
    box.vm.network "private_network", ipconf
    end
    
    if boxconfig.key?(:public)
    box.vm.network "public_network", boxconfig[:public]
    end

    box.vm.provision "shell", inline: <<-SHELL
    mkdir -p ~root/.ssh
    cp ~vagrant/.ssh/auth* ~root/.ssh
    SHELL
        
    case boxname.to_s
    when "iR"
    box.vm.provision "shell", run: "always", inline: <<-SHELL
      sysctl net.ipv4.conf.all.forwarding=1
     # iptables -t nat -A POSTROUTING ! -d 192.168.0.0/16 -o eth0 -j MASQUERADE
      iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
     # ip route add 10.10.10.0/24 via 192.168.255.2
      SHELL
    when "cR"
    box.vm.provision "shell", run: "always", inline: <<-SHELL
      sysctl net.ipv4.conf.all.forwarding=1	
      sysctl -p
      SHELL
    when "tC1"
    box.vm.provision "shell", run: "always", inline: <<-SHELL
      SHELL
      end
    config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.become = "true"
     end
    end
   end


end
