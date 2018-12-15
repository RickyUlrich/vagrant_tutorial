
# Devops Part 1 - Vagrant, the gateway into devops


## Slide 1 - What is a `vagrant` and why do I want one?
- "Vagrant provides `easy to configure`, `reproducible`, and `portable` work
  environments built on top of industry-standard technology and controlled by a
  single consistent workflow to help maximize the productivity and
  flexibility of you and your team." - (https://www.vagrantup.com/intro/)
- "If you are a developer, Vagrant will `isolate dependencies` and their
  configuration within a `single disposable, consistent environment`,
  without sacrificing any of the tools you are used to working
  with (editors, browsers, debuggers, etc.)."
- "If you are an operations engineer or DevOps engineer, Vagrant gives
  you a `disposable environment` and `consistent workflow` for developing
  and testing infrastructure management scripts."


## Slide 2 - The problem
```
ricky$ ls -lh ~/VirtualBox\ VMs/gui_default_1544896071341_40321
/
total 12004888
drwx------  4 ricky  staff   128B Dec 15 13:09 Logs
-rw-------  1 ricky  staff   5.7G Dec 15 13:31 debian-9.5-amd64-disk001.vmdk
-rw-------  1 ricky  staff   4.2K Dec 15 13:09 gui_default_1544896071341_40321.vbox
```
- How do you transfer a VM that is potentially GBs in size?

- Some temporary fixes that come to mind: 
    1. Tranfer whole VM
    2. Give a recipe (ie. Start with this vm, then do x, y, z)


## Slide 3 - Solution
- Vagrant provides a layer of abstraction for creating and provision vms
- A Vagranfile is a `recipe` for deploying a vm on many providers
    - virtualbox
    - amazon
    - docker
- A Vagrantfile consists of three things:
    - System properties (base os layer, architecture, RAM CPUs, network config)
    - Dependencies (a script(s) for specifying what software to install)
    - A set of deployment configs (think config for web server)

## Slide 4 - Usage (demo)
- Create a `Vagrantfile` from a known template
- `vagrant up` - Start/provision the vm
- `vagrant ssh` - Use the vm
- `vagrant halt` - stop vm
- `vagrant up` - Start vm
- `vagrant destroy` - Remove VM for good 

## Slide 5 - Anatomy of a Vagrantfile
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

## Slide 6 - Lessons learned
- VMs are like baking
    - It's hard to understand how something was made from simply the final product
    - Vagrant forces us to come up with "recipes"
      to make it easier to transfer environments for running/deploying code

- A `Vagrantfile` is essentially a recipe
    1. System properties (base layer, architecture, network configuration)
    2. Dependencies (what binaries to install or build on system)
    3. Runtime configuration (what config files to bring to the sytem)

- Finally, Vagrant is not the perfect, end-all-be-all solution for automation.
    - Vagrant serves as an acessible way to get introduced to fundamental 
      ideas in devops
    - Vagrant also give developers flexibilty in sharing/spinning up VMs.
