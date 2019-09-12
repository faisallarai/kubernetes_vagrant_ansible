
domain = 'kube'

nodes = [
  {
    :hostname => 'master-node',
    :ip => '10.0.0.10',
    :id => '10',
    :port => '2210',
    :memory => 2048
  },
  {
    :hostname => 'worker-node1',
    :ip => '10.0.0.11',
    :id => '11',
    :port => '2211',
    :memory => 2048
  },
  {
    :hostname => 'worker-node2',
    :ip => '10.0.0.12',
    :id => '12',
    :port => '2212',
    :memory => 1024
  },
  {
    :hostname => 'worker-node3',
    :ip => '10.0.0.13',
    :id => '13',
    :port => '2213',
    :memory => 1024
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
      nodeconfig.disksize.size = '30GB'
      nodeconfig.vm.hostname = node[:hostname]
      nodeconfig.vm.boot_timeout = 300
      nodeconfig.vm.network :forwarded_port, guest: 22, host: node[:port], id: "ssh", auto_correct: true
      nodeconfig.vm.network :private_network, ip: node[:ip]
      nodeconfig.vm.provider :virtualbox do |vb|
        vb.name = node[:hostname]+"."+domain
        vb.memory = node[:memory]
        vb.cpus = 2
        vb.gui = false
        #vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
        #vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
        vb.customize ['modifyvm', :id, '--macaddress1', "5CA1AB1E00"+node[:id]]
        #vb.customize ['modifyvm', :id, '--natnet1', "192.168/16"]
      end
      nodeconfig.vm.provision "file", source: "hosts", destination: "hosts"
      nodeconfig.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "/home/vagrant/.ssh/id_rsa"
      nodeconfig.vm.provision "shell", inline: $script
      #nodeconfig.vm.provision "ansible" do |ansible|
      #  ansible.playbook = "provision/playbook.yml"
      #  ansible.extra_vars = {
      #    node_ip: node[:ip],
      #  }
      #  ansible.groups = {
      #    "master" => ["master-node"],
      #    "worker" => ["worker-node1","worker-node2","worker-node3"]
      #  }
      #end
    end
  end
end
