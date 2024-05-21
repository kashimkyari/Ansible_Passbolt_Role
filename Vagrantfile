Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.roles_path = "roles"
    ansible.extra_vars = {
      passbolt_db_password: "example_password"
    }
  end
end
