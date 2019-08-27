
domain = 'kube'

nodes = [
  {
    :hostname => 'master',
    :ip => '10.0.0.10',
    :id => '10'
  },
  {
    :hostname => 'node1',
    :ip => '10.0.0.11',
    :id => '11'
  },
  {
    :hostname => 'node2',
    :ip => '10.0.0.12',
    :id => '11'
  },
  {
    :hostname => 'node3',
    :ip => '10.0.0.13',
    :id => '10'
  }
]

$script = <<SCRIPT
sudo mv hosts /etc/hosts
chmod 0600 /home/vagrant/.ssh/id_rsa
usermod -a -G vagrant ubuntu
cp -Rvf /home/vagrant/.ssh /home/ubuntu
chown -Rvf ubuntu /home/ubuntu
apt-get -y update
swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
apt-get -y install python-minimal python-apt
SCRIPT

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  nodes.each do |node|
    config.vm.define node[:hostname] do |nodeconfig|
      nodeconfig.vm.box = "ubuntu/bionic64"
      nodeconfig.vm.hostname = node[:hostname]
      nodeconfig.vm.network :private_network, ip: node[:ip], virtualbox__intnet: domain
      nodeconfig.vm.provider :virtualbox do |vb|
        vb.name = node[:hostname]+"."+domain
        vb.memory = 1024
        vb.cpus = 1
        vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
        vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
        vb.customize ['modifyvm', :id, '--macaddress1', "5CA1AB1E00"+node[:id]]
        vb.customize ['modifyvm', :id, '--natnet1', "192.168/16"]
      end
      nodeconfig.vm.provision "ansible" do |ansible|
        ansible.playbook = "provision/playbook.yml"
      end
      nodeconfig.vm.provision "file", source: "hosts", destination: "hosts"
      nodeconfig.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "/home/vagrant/.ssh/id_rsa"
      nodeconfig.vm.provision "shell", inline: $script
    end
  end
end
