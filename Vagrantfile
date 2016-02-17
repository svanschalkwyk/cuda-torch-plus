# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.provision "docker" do |d|
    d.pull_images "kaixhin/cuda-torch"
    config.vm.image = "kaixhin/cuda-torch"
  end
  config.vm.provision "virtualbox" do |d|
     config.vm.box = "ubuntu/wily64" #MAINTAINER Kai Arulkumaran <design@kaixhin.com>
  end
 
  config.vm.box_check_update = false
  #config.vm.network "forwarded_port", guest: 8888, host: 9888
  #config.vm.network "forwarded_port", guest: 22, host: 9022

  config.vm.network "public_network", ip: "192.168.4.112", bridge: "enp5s0f1"
  config.vm.network "public_network", bridge: "wlxc83a35cc825f"

  config.vm.synced_folder "~/itorch_notebooks", "/home/vagrant/itorch_notebooks"
 config.vm.synced_folder "~/ipython_notebooks", "/home/vagrant/ipython_notebooks"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = "16384"
    vb.name = "iTorch"
    vb.cpus = "4"
  end


$script= <<-SCRIPT
	# Install latest versions of nn, cutorch and cunn
	  luarocks install nn && \
	  luarocks install cutorch && \
	  luarocks install cunn

	# Install CUDA repo (needed for cuDNN)
	export CUDA_REPO_PKG=cuda-repo-ubuntu1404_7.5-18_amd64.deb
	wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/$CUDA_REPO_PKG && \
	  dpkg -i $CUDA_REPO_PKG

	# Install cuDNN v4
	export ML_REPO_PKG=nvidia-machine-learning-repo_4.0-2_amd64.deb
	wget http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1404/x86_64/$ML_REPO_PKG && \
	  dpkg -i $ML_REPO_PKG && \
	  apt-get update && apt-get install -y libcudnn4
	# Install Torch cuDNN bindings
	cd /root && git clone https://github.com/soumith/cudnn.torch.git && cd cudnn.torch && \
	  git checkout R4 && \
	  luarocks make

	# Install NCCL for multi-GPU communication
	cd /root && git clone https://github.com/NVIDIA/nccl.git && cd nccl && \
		 make CUDA_HOME=/usr/local/cuda -j"$(nproc)" && \
		 make PREFIX=/root/nccl -j"$(nproc)" install
		 export LD_LIBRARY_PATH=/root/nccl/lib:$LD_LIBRARY_PATH
	luarocks install nccl

	# Install luaposix
	luarocks install luaposix

	# Install Moses
	luarocks install moses

	# Install cjson
	luarocks install lua-cjson

	# Install LogRoll
	luarocks install logroll

	# Install ffmpeg dependencies
	add-apt-repository ppa:mc3man/trusty-media && \
	  apt-get update && apt-get install -y ffmpeg
	# Install ffmpeg
	luarocks install ffmpeg

	# Install LuaSocket
	luarocks install luasocket

	# Install parallel
	luarocks install parallel

	# Install IPC
	luarocks install ipc

	# Install distlearn
	luarocks install distlearn

	# Install classic
	luarocks install classic

	# Install torchx
	luarocks install torchx

	# Install autograd
	luarocks install autograd

	# Install dataset
	luarocks install dataset

	# Install dpnn
	luarocks install dpnn

	# Install nninit
	luarocks install nninit

	# Install nnquery
	luarocks install https://raw.githubusercontent.com/bshillingford/nnquery/master/rocks/nnquery-scm-1.rockspec

	# Install rnn
	luarocks install rnn

	# Install loadcaffe dependencies
	apt-get install -y \
	  libprotobuf-dev \
	  protobuf-compiler
	# Install loadcaffe
	luarocks install loadcaffe

	# Install inn
	luarocks install inn

	# Install stn
	luarocks install https://raw.githubusercontent.com/qassemoquab/stnbhwd/master/stnbhwd-scm-1.rockspec

	# Reinforcement learning

	# Install xitari
	luarocks install https://raw.githubusercontent.com/Kaixhin/xitari/master/xitari-0-0.rockspec

	# Install alewrap
	luarocks install https://raw.githubusercontent.com/Kaixhin/alewrap/master/alewrap-0-0.rockspec

	# Install rlenvs
	luarocks install https://raw.githubusercontent.com/Kaixhin/rlenvs/master/rocks/rlenvs-scm-1.rockspec
SCRIPT

	config.vm.provision "shell", inline: $script, privileged: true

end
    
