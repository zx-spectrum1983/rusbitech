Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2004"

  config.vm.provider :vmware_esxi do |esxi|
    esxi.esxi_hostname = '192.168.1.100'
    esxi.esxi_username = 'root'
    esxi.esxi_password = 'key:~/.ssh/id_rsa'
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt update
    apt install git ansible -y
    git clone https://github.com/zx-spectrum1983/rusbitech.git & GIT_PID=$!
    wait $GIT_PID
    cd rusbitech
    ansible-playbook --connection=local --inventory 127.0.0.1, playbooks/play-role.yml -e "ROLE=90-install-docker facts=false"
    ansible-playbook --connection=local --inventory 127.0.0.1, playbooks/play-role.yml -e "ROLE=91-install-node_exporter facts=false"
    ansible-playbook --connection=local --inventory 127.0.0.1, playbooks/play-role.yml -e "ROLE=92-deploy-monitoring facts=false"
    ansible-playbook --connection=local --inventory 127.0.0.1, playbooks/play-role.yml -e "ROLE=93-configure-grafana facts=true"
  SHELL

end
