# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.network :forwarded_port, guest: 3306, host: 3306
  config.vm.network :forwarded_port, guest: 3000, host: 3000
  config.vm.network :private_network, ip: "192.168.10.10"
  config.vm.synced_folder ".", "/vagrant", id: "vagrant-root", nfs: true 

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "cookbooks"
    chef.add_recipe "apt"
    chef.add_recipe "build-essential"
    chef.add_recipe "nfs"
    chef.add_recipe "rvm::vagrant"
    chef.add_recipe "rvm::gem_package"
    chef.add_recipe "rvm::system"
    chef.add_recipe "git"
    chef.add_recipe "xvfb"
    chef.add_recipe "qt"
    chef.add_recipe "mysql"
    chef.add_recipe "mysql::server"
    chef.json = {
      rvm: {
        group_users: ["vagrant"],
        default_ruby: "ruby-2.0.0-p247",
        user_installs: [ 
          { 
            default_ruby: "ruby-2.0.0-p247@project",
            user: 'vagrant',
            rvmrc: {
              rvm_project_rvmrc: 1,
              rvm_gemset_create_on_use_flag: 1,
              rvm_pretty_print_flag: 1
            }
          }
        ]
      },
      mysql: {
        server_root_password: "",
        server_repl_password: "",
        server_debian_password: "",
        client: { 
          packages: ["mysql-client", "libmysqlclient-dev","ruby-mysql"] 
        }
      }
    }
  end
end