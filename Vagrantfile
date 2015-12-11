Vagrant.require_version ">= 1.7.0"

Vagrant.configure(2) do |config|

  config.vm.box = "centos/7"

  # Disable the new default behavior introduced in Vagrant 1.7, to
  # ensure that all Vagrant machines will use the same SSH key pair.
  # See https://github.com/mitchellh/vagrant/issues/5005
  config.ssh.insert_key = false

  config.vm.synced_folder ".", "/home/vagrant/sync/", disabled: true

  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "test/playbook.yml"
    ansible.vault_password_file = ".vault_password"
  end
end
