# Start with command: vagrant up --provision
Vagrant.configure("2") do |config|
  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--cableconnected1", "on"] # solving issue: freeze at ssh auth method private key
    v.customize ["modifyvm", :id, "--uart1", "0x3F8", "4"] # solving issue: freeze at ssh auth method private key
    v.customize ["modifyvm", :id, "--uartmode1", "file", File::NULL] # solving issue: freeze at ssh auth method private key
    v.customize [
      "modifyvm", :id,
      "--memory", 512,
      "--cpus", 1,
      ]
  end
  config.vm.boot_timeout = 100

  config.vm.define "proxy" do |proxy|
    proxy.vm.box = "ubuntu/focal64"
    proxy.vm.hostname = "proxy"
    proxy.vm.network "private_network", ip: "192.168.111.10"
    proxy.vm.network :forwarded_port, guest: 80, host: 1234
  end

  config.vm.define "server1" do |server1|
    server1.vm.box = "ubuntu/focal64"
    server1.vm.hostname = "server1"
    server1.vm.network "private_network", ip: "192.168.111.11"
  end

  config.vm.define "server4" do |server4|
    server4.vm.box = "ubuntu/focal64"
    server4.vm.hostname = "server4"
    server4.vm.network "private_network", ip: "192.168.111.12"
  end

  config.vm.define "server3" do |server3|
    server3.vm.box = "ubuntu/focal64"
    server3.vm.hostname = "server3"
    server3.vm.network "private_network", ip: "192.168.111.13"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "ansible/playbook.yml"
  end

end