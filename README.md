
# Devops Part 1 - Vagrant, the gateway into devops


## Slide 1 - What is Devops


## Slide 2 - What is a 'vagrant' and why do I want one?


## Slide 3 - The problem
- How do you transfer a VM that is potentially GBs in size?

- What people do?
    1. Tranfer whole vm
    2. Give a recipe (ie. Start with this vm, then do x, y, z)


## Slide 4 - Solution
- Vagrant provides a layer of abstraction for creating and provision vms
- A Vagranfile is a deployment of vm on many different providers
    - virtualbox
    - amazon
    - docker
- A Vagrantfile consists of three things
    - System properties (base os layer, architecture, RAM CPUs, network config)
    - Dependencies (a script(s) for specifying what software to install)
    - A set of deployment configs (think config for web server)

## Slide 5 - Usage
- Create a `Vagrantfile` from a known template (root dir of project)
- `vagrant up` - Start/provision the vm
- `vagrant ssh` - Use the vm
- `vagrant halt` - stop vm
- `vagrant up` - Start vm
- `vagrant destroy` - Remove VM for good 

## Slide 6 - Anatomy of a Vagrantfile
```
Vagrant.configure("2") do |config|

  # Recipe Part 1 - OS base layer, memory limit, cpus
  config.vm.box = "bento/debian-9.5"
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", "2024"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]
  end

  # Recipe Part 2 - Provision dependencies for software
  config.vm.provision "shell", inline: <<-SHELL
    export DEBIAN_FRONTEND=noninteractive

    apt-get update -y && apt-get upgrade -y

    # random stuff
    apt-get install -y python-pip python3-pip git build-essential vim git tmux
  SHELL

  # Recipe Part 3 - Apply software configs
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    # empty for now
  SHELL

end
```

## Slide 7 - Lessons learned
- VMs are like baking
    - it's hard to understand how something was made from simply the final product
    - Vagrant and devops forces us to come up with "recipes" to make it easier to transfer environments for running/deploying code

- A `Vagrantfile` is essentially a recipe
    1. System properties (base layer, architecture, network configuration)
    2. Dependencies (what binaries to install or build on system)
    3. Runtime configuration (what config files to bring to the sytem)

- Finally, Vagrant is not the perfect, end-all-be-all solution for automation.
    - Vagrant serves as an acessible way to get introduced to fundamental 
      ideas in devops
    - Vagrant also give developers flexibilty in sharing/spinning up VMs.
