VAGRANTFILE_API_VERSION = 2

name = "MEAN-dev"
memory = "512"
cpu="2"
type="nfs" # "", "nfs"
ip = "192.168.56.4"
home = "/home/vagrant/project"
sync= "."

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.hostname = 'mean-dev'
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: ip

  # nfs :
  #
  # windows :  vagrant plugin install vagrant-winnfsd
  # ubuntu  : sudo apt-get install nfs-kernel-server

  if type
    config.vm.synced_folder sync, home, type: type
  else
    config.vm.synced_folder sync, home
  end
  # config.vm.synced_folder ".", "/home/vagrant/project", type: "nfs"

  # SSH Agent Forwarding
  #
  # Enable agent forwarding on vagrant ssh commands. This allows you to use ssh keys
  # on your host machine inside the guest. See the manual for `ssh-add`.

  #  config.ssh.private_key_path = '~/.ssh/id_rsa'
  #  config.ssh.forward_agent = true

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |v|
  #    #   v.customize ["modifyvm", :id, "--memory", memory]
  # end
   config.vm.provider "virtualbox" do |vb|
     # Display the VirtualBox GUI when booting the machine
     # vb.gui = true
     vb.name = name
     # Customize the amount of memory on the VM:
     vb.memory = memory
     vb.customize ["modifyvm", :id, "--cpus", cpu]
     vb.customize ["modifyvm", :id, "--vram", "8"]
     vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
   end

  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL

    ##update
    sudo apt-get -y update

    ##
    apt-get install -y apache2 libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev

    ##bash-completion
    sudo dpkg -l | grep bash-completion || ( sudo apt-get install -y bash-completion )

    ##git
    which git || sudo apt-get install -y git
    
	## global aliases (just copy and paste line to command prompt)
    # git config --global alias.co checkout && git config --global alias.br branch && git config --global alias.ci commit && git config --global alias.st status && git config --global alias.hist 'log --pretty=format:"%h %ad | %s%d [%an]" --graph --date=short' && git config --global alias.type 'cat-file -t' && git config --global alias.dump 'cat-file -p'

	## config aliases for git, vagrant and npm
    ## $ shopt -s expand_aliases, $ source ~/.bash_aliases

    # for A in "alias ns='npm start '" "alias gs='git status '" "alias ga='git add '" "alias gA='git add -A'" "alias gb='git branch '" "alias gc='git commit'" "alias gd='git diff'" "alias go='git checkout '" "alias gk='gitk --all&'" "alias gx='gitx --all'" "alias got='git '" "alias get='git '"; do
    #   echo ${A} >> ~/.profile; # or ~/.bash_aliases
    #   echo >> ~/.profile; # ~/.bash_aliases
    # done

    ##curl
    which curl || (sudo apt-get install -y curl libcurl3 libcurl3-dev php5-curl && sudo service apache2 restart)

    ##nodejs

    which npm ||
    (sudo apt-get -y install python-software-properties python && curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
  sudo apt-get install -y nodejs && sudo apt-get install -y build-essential &&  apt-get install -y nodejs-legacy)

	## nvm
	## which nvm ||	(curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.29.0/install.sh | bash && source ~/.profile && nvm use system)

    ##nodemon
    which nodemon || sudo npm install -g nodemon

    ##bower
    which bower || sudo npm install -y bower -g

    ##mongo
    which mongo ||
    (sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10 && echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list && sudo apt-get update -y && sudo apt-get install -y mongodb-org && sudo locale-gen en_US.UTF-8 && sudo dpkg-reconfigure locales && export LC_ALL=en_US.UTF-8)

	## if main OS is Windows
	## which dos2unix || sudo apt-get -y install dos2unix

  SHELL

end
