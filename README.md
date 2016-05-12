# vagrant + yaml setup

## Finished Product

This config will result in a virtual machine running puppet server with a locally accessible IP of 10.25.0.10. The 10.25.0.0 range was chosen because it is a relatively obscure range that will hopefully not conflict with other ranges.

### Ports
| Service | IP | Port
| ------- | -- | ----
Puppet Server | 10.25.0.10 | 8140

### Sudo

By design and default the vagrant user has sudo access without requiring a password.

## Quick start

1. Pull the Puppet module dependencies
> This will pull all of the Puppet modules required.

`git submodule update --init --recursive`
2. Start the virtual machine
> This will create the virtual machine and run the initial provisioning including the internal network.

`vagrant up`
3. Re puppet the virtual machine
> We need to re-apply the Puppet after the initial run now that the internal networks are sorted properly.

`vagrant provision --provision-with puppet`
4. Implement SSH workaround for Windows
> Unfortunately vagrant ssh doesn't work on Windows. There is a simple workaround.

`vagrant ssh-config > config`

> Now we can SSH into a virtual machine using:

`ssh -F config <virtual machine name>`


## Requirements

1. Install [virtualbox](https://www.virtualbox.org/wiki/Downloads)
2. Install [vagrant](https://www.vagrantup.com/downloads.html)
3. Install vagrant-cachier `vagrant plugin install vagrant-cachier`
> This is a nice to have, it helps speed up the provisioning process if you're constantly destroying and recreating VMs

4. Install vagrant-hosts `vagrant plugin install vagrant-hosts`
> This provides a very simple poor man's DNS for your vagrant environment. VERY helpful for puppet.

## Adding new virtual machines
> This is a simple process, we just need to add the new VM definition into machines.yaml

1. Edit the machines.yaml file
2. Duplicate the array used for the Puppetmaster VM
3. Give the new VM a name, an internal IP and appropriate RAM and CPU
4. Start the VM
`vagrant up <new vm name>`

## Supported providers

Currently only supports virtualbox because I am lazy. With a little effort all other supported providers will work.
