Vagrant.configure("2") do |config|

  # MASTER test box
  config.vm.define "master" do |master|
    master.vm.box = "ubuntu/xenial64"
    master.vm.network :private_network, ip: "192.168.33.9"
    master.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "master"]
    end
  end

  # BACKUP test box
  config.vm.define "backup" do |backup|
    backup.vm.box = "ubuntu/xenial64"
    backup.vm.network :private_network, ip: "192.168.33.10"
    backup.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--name", "backup"]
    end
  end


  # both boxes - install python
  config.vm.provision "shell" do |shell|
    shell.inline = "sudo --preserve-env apt update; sudo --preserve-env apt install -y python"
    shell.env = {"http_proxy": "#{ENV['http_proxy']}"}
  end

  # both boxes - run playbook
  config.vm.provision :ansible do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.galaxy_roles_path = ".vagrant/roles"
    # ansible.galaxy_role_file = "requirements.yml"
    ansible.limit = ""
  end

end
