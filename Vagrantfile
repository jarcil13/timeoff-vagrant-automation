Vagrant.configure("2") do |config|
    config.vm.define "app1" do |app|
      app.vm.box = "ubuntu/bionic64"
      app.vm.network "forwarded_port", guest: 3000, host: 8080
      app.vm.network "private_network", ip: "192.168.56.2"
      app.vm.provider "virtualbox" do |v|
        v.name = "app1"
       end
    end

    config.vm.define "jenkins-master" do |jen|
        jen.vm.box = "ubuntu/bionic64"
        jen.vm.network "forwarded_port", guest: 8080, host: 8081
        jen.vm.network "private_network", ip: "192.168.56.3"
        jen.vm.provider "virtualbox" do |v|
          v.name = "jenkins-master"
          v.memory = "2048"
         end
        jen.vm.provision "ansible_local" do |ansible|
            ansible.config_file         = "provisioning/config/ansible.cfg"
            ansible.playbook            = "provisioning/site.yml"
            ansible.inventory_path      = "provisioning/inventory/prod"
            ansible.limit               = "all"
            ansible.galaxy_roles_path   = "/etc/ansible/roles"
            ansible.galaxy_role_file    = "provisioning/config/galaxy_requirements.yml"
            ansible.galaxy_command      = "sudo ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path}"
            ansible.vault_password_file = "vault-prd" # Vault encryption is used since sentitive info is available on groups vars, pwd saved locally. 
        end
    end

  end