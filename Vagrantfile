VAGRANTFILE_API_VERSION = "2"
DOMAIN = 'domain.com'
BOX = 'precise64'
BOX_URL = 'http://files.vagrantup.com/precise64_vmware.box'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = BOX
  config.vm.box_url = BOX_URL

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = false

  config.vm.define :salt, primary: true do |salt_master|
    salt_master.vm.box = BOX
    salt_master.vm.box_url = BOX_URL

    salt_master.vm.hostname = "salt"
    salt_master.vm.network :private_network, ip: '192.168.50.100'
    salt_master.hostmanager.aliases = ["salt.#{DOMAIN}"]

    salt_master.vm.provision :salt do |salt|
      salt.install_type = 'stable'
      salt.install_master = true
      salt.verbose = true
      salt.minion_config = 'salt/minion'
      salt.master_config = 'salt/master'
    end
  end

  config.vm.define :salt_minion_1, primary: false do |salt_minion_1|
    salt_minion_1.vm.box = BOX
    salt_minion_1.vm.box_url = BOX_URL

    salt_minion_1.vm.hostname = "minion-1"
    salt_minion_1.vm.network :private_network, ip: '192.168.50.101'
    salt_minion_1.hostmanager.aliases = ["minion-1.#{DOMAIN}"]

    salt_minion_1.vm.provision :salt do |salt|
      salt.install_type = 'stable'
      salt.install_master = false
      salt.verbose = true
      salt.minion_config = 'salt/minion'
      salt.master_config = 'salt/master'
    end
  end

  config.vm.define :salt_minion_2, primary: false do |salt_minion_2|
    salt_minion_2.vm.box = BOX
    salt_minion_2.vm.box_url = BOX_URL

    salt_minion_2.vm.hostname = "minion-2"
    salt_minion_2.vm.network :private_network, ip: '192.168.50.102'
    salt_minion_2.hostmanager.aliases = ["minion-2.#{DOMAIN}"]

    salt_minion_2.vm.provision :salt do |salt|
      salt.install_type = 'stable'
      salt.install_master = false
      salt.verbose = true
      salt.minion_config = 'salt/minion'
      salt.master_config = 'salt/master'
    end
  end

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  # config.vm.box_url = "http://domain.com/path/to/above.box"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network :forwarded_port, guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network :private_network, ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network :public_network

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider :virtualbox do |vb|
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxManage to customize the VM. For example to change memory:
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  # end
  #
  # View the documentation for the provider you're using for more
  # information on available options.

  # Enable provisioning with Puppet stand alone.  Puppet manifests
  # are contained in a directory path relative to this Vagrantfile.
  # You will need to create the manifests directory and a manifest in
  # the file base.pp in the manifests_path directory.
  #
  # An example Puppet manifest to provision the message of the day:
  #
  # # group { "puppet":
  # #   ensure => "present",
  # # }
  # #
  # # File { owner => 0, group => 0, mode => 0644 }
  # #
  # # file { '/etc/motd':
  # #   content => "Welcome to your Vagrant-built virtual machine!
  # #               Managed by Puppet.\n"
  # # }
  #
  # config.vm.provision :puppet do |puppet|
  #   puppet.manifests_path = "manifests"
  #   puppet.manifest_file  = "site.pp"
  # end

  # Enable provisioning with chef solo, specifying a cookbooks path, roles
  # path, and data_bags path (all relative to this Vagrantfile), and adding
  # some recipes and/or roles.
  #
  # config.vm.provision :chef_solo do |chef|
  #   chef.cookbooks_path = "../my-recipes/cookbooks"
  #   chef.roles_path = "../my-recipes/roles"
  #   chef.data_bags_path = "../my-recipes/data_bags"
  #   chef.add_recipe "mysql"
  #   chef.add_role "web"
  #
  #   # You may also specify custom JSON attributes:
  #   chef.json = { :mysql_password => "foo" }
  # end

  # Enable provisioning with chef server, specifying the chef server URL,
  # and the path to the validation key (relative to this Vagrantfile).
  #
  # The Opscode Platform uses HTTPS. Substitute your organization for
  # ORGNAME in the URL and validation key.
  #
  # If you have your own Chef Server, use the appropriate URL, which may be
  # HTTP instead of HTTPS depending on your configuration. Also change the
  # validation key to validation.pem.
  #
  # config.vm.provision :chef_client do |chef|
  #   chef.chef_server_url = "https://api.opscode.com/organizations/ORGNAME"
  #   chef.validation_key_path = "ORGNAME-validator.pem"
  # end
  #
  # If you're using the Opscode platform, your validator client is
  # ORGNAME-validator, replacing ORGNAME with your organization name.
  #
  # If you have your own Chef Server, the default validation client name is
  # chef-validator, unless you changed the configuration.
  #
  #   chef.validation_client_name = "ORGNAME-validator"
end
