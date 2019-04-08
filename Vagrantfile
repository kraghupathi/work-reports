# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
# Every Vagrant development environment requires a box. You can search for
# boxes at https://vagrantcloud.com/search.
  config.vm.box = "kraghupathi/box"
  config.ssh.insert_key = 'false'
# config.vm.base_mac = nil

# Customize the amount of HDD
  config.disksize.size = '100GB'

# Customize the amount of memory on the VM:
config.vm.provider :virtualbox do |vb|
    vb.memory = 16240
    vb.cpus = 4
end

# Create a forwarded port mapping which allows access to a specific port
# within the machine from a port on the host machine and only allow access
# via x.x.x.x to disable public access
  config.vm.network "forwarded_port", guest: 80, host: xx
  config.vm.network "private_network", ip: "x.x.x.x", nic2_type: "eth1"

# Configure VM post message
config.vm.post_up_message = <<-MESSAGE
    Welcome to BOX!
    Run "vagrant ssh" to get started.
  MESSAGE
end
