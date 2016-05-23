Vagrant.configure("2") do |config|
#config.vm.box = "bento/centos-6.7"
config.vm.box = "hashicorp/precise64"

  config.vm.provision "fix-no-tty", type: "shell" do |s|
       s.privileged = false
       s.inline = "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile"
  end

  config.vm.define "web01" do |web01|
	web01.vm.network :private_network, ip: "10.10.10.101"
	web01.vm.hostname = 'web01'
  end

  config.vm.define "web02" do |web02|
        web02.vm.network :private_network, ip: "10.10.10.102"
        web02.vm.hostname = 'web02'
  end
	
  config.vm.define "lb" do |lb|
        lb.vm.network :private_network, ip: "10.10.10.100"
        lb.vm.hostname = 'lb'
  end

  config.vm.define "monitoring" do |mon|
        mon.vm.network :private_network, ip: "10.10.10.103"
        mon.vm.hostname = 'monitoring'
  end

  #
  # Run Ansible from the Vagrant Host
  #
  config.vm.provision "ansible" do |ansible|
    ansible.groups = {
      "webservers" => ["web01", "web02"],
      "load_balancer" => ["lb"],
      "monitoring" => ["monitoring"],
    }
    ansible.playbook = "provision.yml"
  end
end
