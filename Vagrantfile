Vagrant.configure("2") do |config|
  config.vm.define "web" do |web|
    web.vm.box = "generic/centos8"
    web.vm.network "private_network", ip: "192.168.56.66"
    web.vm.hostname = "web"
    web.vm.provider "virtualbox" do |vb|
      vb.name = "web"
      vb.memory = "2048"
      vb.cpus = "2"
    end

    web.vm.provision "setup-deployment-user", type: "shell" do |s|
      ssh_pub_key = File.readlines("./client.pem.pub").first.strip
      s.inline = <<-SHELL
          # create devops user
          useradd -s /bin/bash -d /home/devops/ -m -G wheel devops
          echo 'devops ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers
          mkdir -p /home/devops/.ssh && chown -R devops /home/devops/.ssh
          echo #{ssh_pub_key} >> /home/devops/.ssh/authorized_keys
          chmod 700 /home/devops/.ssh
          sudo chmod 640 /home/devops/.ssh/authorized_keys
          sudo chown devops /home/devops/.ssh
          sudo chown devops /home/devops/.ssh/authorized_keys
      SHELL
    end
  end
end
