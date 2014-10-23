title: Vagrant Brown Bag
date: 2014-10-23
author:
    name: Peter Eichman
    email: peichman@umd.edu
    url: https://github.com/peichman-umd/vagrant-brown-bag
output: presentation.html

--

# Vagrant

## Introduction and Basic Concepts

--

### What is Vagrant?

- Source control for VMs

<!--

Or, as the creator of Vagrant puts it more broadly, source control for
(development) environments

/-->

--

### What are its moving parts?

- Provider
- Box
- Vagrantfile
- Provisioning scripts

--

### What is a provider?

- Virtualization host
- VirtualBox is the default
- Also supports Docker and Hyper-V
- Plugins for other providers, both local and cloud:
    - [VMWare](http://www.vagrantup.com/vmware)
    - [Amazon AWS](https://github.com/mitchellh/vagrant-aws)
    - [...and many more](https://github.com/mitchellh/vagrant/wiki/Available-Vagrant-Plugins#providers)

--

### What are boxes?

- Base VM image
- Contains a metadata JSON identifiying the provider
- Possibly a Vagrantfile with some defaults
- Beyond that, each provider has its own box file format
- Community boxes at [VagrantCloud](https://vagrantcloud.com/)

<!--

For providers like VirtualBox or VMWare, the box contains an exported virtual
machine (OVF or VMX file, plus the VMDK files for the virtual disks.

For cloud providers like Amazon AWS or Rackspace, the box is usually just has a
basic Vagrantfile that sets up some defaults.

You can create a box from a running Vagrant environment by using vagrant
package.

More detailed management of the creation of boxes is outside of Vagrant's scope;
use a tool like Packer instead to script the creation of boxes

/-->

--

### What is a Vagrantfile?

- Basic configuration of the VM
- Choose the base box
- Network and SSH setup
- Shared folders
- What provisioners to run
- Some providers also allow you to tweak things like RAM or HD size

<!--

Vagrantfile inheritence: box -> project

/-->

--

### What are provisioning scripts?

- The way to install and configure software on the VM
- Shell, Puppet, Chef, etc.

--

### What are its advantages?

- Quick deployment: `git clone && vagrant up`
- Consistent setup for all developers/users
- Isolate your workstation from stuff you are experimenting with
- Configuration as source controlled code

--

### What are its uses?

- Development environments
- Demos
- Testing, staging, and production environments?

<!--

It may be possible or beneficial to share provisioning information between
development, staging, and production environments, especially if using something
like Puppet.

/-->

--

### Networking

- Port forwarding
- Private IP address

<!--

By default, Vagrant will find an open port to forward to 22 on the guest for SSH
connections.

For development environments, I tend to use a private IP address, usually
something of the form 192.168.xxx.10, where xxx veries by project. Then I add a
mapping to that address in my workstation's /etc/hosts file

Bridged network to place it on a public network.

/-->

--

### Shared Folders

- Vagrantfile directory ⇒ `/vagrant`
- Can define additional shared folders
- For development, share your local working copy with the VM: `../` ⇒
  `/apps/git/myappname`

<!--

Local providers use the VM's shared folder mechanism to mount the shared
folders. Non-local providers usually use rsync or a similar method on vagrant up
or reload, so the sharing isn't quite as "live" as local.

/-->

--

### SSH Access

- Maps port 22 on the VM
- `vagrant ssh` (also, `vagrant ssh -- command ...`)
- `vagrant ssh-config`
- Ships with a default keypair
- User `vagrant` with sudo access

--

### Provisioning

- Shell scripts
- File copying
- Declarative provisioners
    - Puppet (Apply and Agent)
    - Chef (Solo and Client)
    - [...and more](http://docs.vagrantup.com/v2/provisioning/index.html)

--

### Making Changes

- Changes to Vagrantfile: `vagrant reload`
- Changes to provisioning scripts: `vagrant provision`
- You can always nuke it from orbit and bring it up again: `vagrant destroy &&
  vagrant up`

<!--

Reloading allows you to change network settings, VM allocation of memory, etc.

Reprovisioning: ideally, your provisioning scripts should be idempotent; this is
most likely the case by default if you are using something like Puppet, but with
a shell script it takes some effort on your part.

/-->

--

### Other Commands

- suspend/resume
- halt/up
- status, global-status
- init
- plugin
- `vagrant help` to view these and more

--

### Related Tools

- [Packer](http://packer.io/): create VM images
- [Docker](http://docker.io/): run applications in containers

<!--

Packer can create images for various virtualization providers. Out of the box,
it supports creating Vagrant boxes.

Docker is about running applications in containers within an existing VM
environment.

/-->

--

### Demos

- [AlephRx](https://github.com/umd-lib/alephrx) development environment
- [Fedora 4](https://github.com/dgcliff/fedora4vagrant) demo

--

### Things to Consider

- Customized base box vs. provisioning scripts
- Syncing development, staging, and production

<!--

Even if we don't use Vagrant to control staging and production environments, it
may be possible to share provisioning scripts.

/-->

--

### Links

- [Vagrant](http://vagrantup.com/)
- [Vagrant Documentation](http://docs.vagrantup.com/v2/)
- [VirtualBox](https://www.virtualbox.org/)
- [VagrantCloud](https://vagrantcloud.com/)

<!-- vim:filetype=markdown
/-->
