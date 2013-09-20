# saltstack-sandbox

A [Vagrant](http://www.vagrantup.com/)-based sandbox environment
for experimenting with [SaltStack](http://saltstack.com/).

## Requirements

saltstack-sandbox has been developed and tested using the following
software versions:

* [Vagrant](http://www.vagrantup.com/) 1.3.3
* [VMWare Fusion](https://my.vmware.com/web/vmware/info/slug/desktop_end_user_computing/vmware_fusion/6_0) 6.0.0

NOTE: VMWare Fusion is a paid product. I was using it because the
      Virtual Box/precise64 combination would consistently
      grey screen my Mac.

## Setup

NOTE: For the commands below, you do NOT need to add the
`--provider vmware_fusion` if you are not using a paid
VM.

Add the `precise64` box to Vagrant:

```sh
$ vagrant box add precise64 --provider vmware_fusion
```

We use the [vagrant-hostmanager](https://github.com/smdahlen/vagrant-hostmanager)
plugin to manage the `/etc/hosts` file for the Vagrant VMs.

Install the plugin using the following command:

```sh
$ vagrant plugin install vagrant-hostmanager
```

You can configure a number of attributes for the cluster
of VMs to be provisioned at the top of the `Vagrantfile`.
The default options are for 3 VMs, `salt`, `minion-1` and
`minion-2`. The default configuration is:

```ruby
CONFIGURATION = {
  domain: 'domain.com',
  box: 'precise64',
  box_url: 'http://files.vagrantup.com/precise64_vmware.box',
  starting_ip_address: '192.168.50.100',
  vms: [
    {
      name: :salt,
      primary: true,
      hostname: 'salt',
      install_master: true
    },
    {
      name: :salt_minion_1,
      primary: false,
      hostname: 'minion-1',
      install_master: false
    },
    {
      name: :salt_minion_2,
      primary: false,
      hostname: 'minion-2',
      install_master: false
    }
  ]
}
```

Run Vagrant to create the VMs:

```sh
$ vagrant up --provider vmware_fusion
```

After successfully running `vagrant up`, you should be able
to SSH into your `salt` box and execute salt to ping the
other VMs in the pool.

```sh
$ vagrant ssh salt
Welcome to Ubuntu 12.04.1 LTS (GNU/Linux 3.2.0-29-virtual x86_64)

 * Documentation:  https://help.ubuntu.com/
Last login: Fri Sep 20 11:56:59 2013 from 192.168.124.1

vagrant@salt:~$ sudo salt '*' test.ping
salt:
    True
minion-1:
    True
minion-2:
    True
vagrant@salt:~$ exit
logout
```

## Notes

* The salt master is setup with `auto_accept: True` in its configuration
  file. As the settings file says, "this setting will automatically
  accept all incoming public keys from the minions. Note that this
  is insecure."

## Copyright

Copyright (c) 2013 David Czarnecki. See LICENSE.txt (MIT license) for
further details.

## Contributing to saltstack-sandbox

* Check out the latest master to make sure the feature hasn't been
  implemented or the bug hasn't been fixed yet
* Check out the issue tracker to make sure someone already hasn't
  requested it and/or contributed it
* Fork the project
* Start a feature/bugfix branch
* Commit and push until you are happy with your contribution
* Please try not to mess with the version or history. If you want
  to have your own version, or is otherwise necessary, that is fine,
  but please isolate to its own commit so I can cherry-pick around it.