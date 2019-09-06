
domain = 'kube'

nodes = [
  {
    :hostname => 'master',
    :ip => '10.0.0.10',
    :id => '10',
    :port => '2210'
  },
  {
    :hostname => 'node1',
    :ip => '10.0.0.11',
    :id => '11',
    :port => '2211'
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
      nodeconfig.vm.network :forwarded_port, guest: 22, host: node[:port], id: "ssh",
    auto_correct: true
      nodeconfig.vm.network :private_network, ip: node[:ip], virtualbox__intnet: domain
      nodeconfig.vm.provider :virtualbox do |vb|
        vb.name = node[:hostname]+"."+domain
        vb.memory = 3072
        vb.cpus = 2
        vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
        vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
        vb.customize ['modifyvm', :id, '--macaddress1', "5CA1AB1E00"+node[:id]]
        vb.customize ['modifyvm', :id, '--natnet1', "192.168/16"]
      end
      nodeconfig.vm.provision "ansible" do |ansible|
        ansible.playbook = "provision/playbook.yml"
        ansible.groups = {
          "master" => ["master"],
          "worker" => ["node1"]
        }
      end
      nodeconfig.vm.provision "file", source: "hosts", destination: "hosts"
      nodeconfig.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "/home/vagrant/.ssh/id_rsa"
      nodeconfig.vm.provision "shell", inline: $script
    end
  end
end
