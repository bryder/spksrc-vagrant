# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "chef/debian-7.6-i386"
  config.vm.provision "file", source: "~/.gitconfig", destination: ".gitconfig"

  #config.vm.synced_folder "spksrc/", "/home/vagrant/spksrc"

  config.vm.hostname = "spksrc"

  #insall chef if not installed on vm
  $provision_script= <<SCRIPT
  if [[ $(which chef-solo) != '/usr/bin/chef-solo' ]]; then
    curl -L https://www.opscode.com/chef/install.sh | sudo bash
    echo 'export PATH="/opt/chef/embedded/bin:$PATH"' >> ~/.bash_profile && source ~/.bash_profile
  fi
SCRIPT
  config.vm.provision :shell, :inline => $provision_script

  #install dependencies
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ['cookbooks', 'site-cookbooks']

    chef.add_recipe("apt")
    chef.add_recipe('build-essential')
    chef.add_recipe('python')
    chef.add_recipe('spksrc')
  end

end
