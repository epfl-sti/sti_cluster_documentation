# Vagrant

Vagrant is a great tool to automate the creattion of virtual machines.

Once you have VirtualBox and Vagrant installed on your machine, it is really quick and easy to create a new VM, manage snapshots of this machine, destroy it and start over.

## Installation

VirtualBox can be found here: [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

Vagrant can be found here: [https://www.vagrantup.com/](https://www.vagrantup.com/)

## Usage

### The Vagrantfile

Every aspects of your virtual machines are managed through a `Vagrantfile`

```ruby
VAGRANTFILE_API_VERSION = "2"

Vagrant.require_version ">= 1.5.0"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

	config.vm.box = "ubuntu/bionic64"

	# Turn off shared folders
	config.vm.synced_folder ".", "/vagrant", disabled: true

	config.vm.provision "shell" do |s|
		ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
		s.inline = <<-SHELL
			echo #{ssh_pub_key} >> /home/ubuntu/.ssh/authorized_keys
		SHELL
	end

	config.vm.define "sti-cluster-node-01.vagrant" do |node4_config|
		node4_config.vm.hostname = "sti-cluster-node-01.vagrant"
		node4_config.vm.network :private_network, ip: "192.168.50.10"
	end
end
```

### Starting your VM(s)

You can start your VM(s) by simply calling:

```bash
vagrant up
```

![vagrant up](../img/vagrant/vagrant_up.gif)

### Connect to your VM via SSH

You can either connect through the built-in command:

```bash
vagrant ssh
```

or via a *regular* ssh command:

```bash
ssh ubuntu@192.168.50.10
```

> it may be useful to change you `~/.ssh/config` file to make sure the fingerprint of the server does not get recorded. It can become quite painful to re-edit your `~/.ssh/known_hosts` every time your re-spawn a VM.

> ```shell
> Host 192.168.50.*
>   StrictHostKeyChecking no
>   UserKnownHostsFile /dev/null
> ```

![vagrant ssh](../img/vagrant/vagrant_ssh.gif)

### Managing snapshots

Managing snaphots is also straightforward.

#### creating a snapshot

```bash
vagrant snapshot save <name>
```

#### restoring the machine to a given snapshot

```bash
vagrant snapshot restore <name>
```

#### deleting a snapshot

```bash
vagrant snapshot delete <name>
```

![vagrant snapshots](../img/vagrant/vagrant_snapshot.gif)

### detroying the VM(s)

Again, this can be easily done by issuing:

```bash
vagrant destroy
```

![vagrant destroy](../img/vagrant/vagrant_destroy.gif)
