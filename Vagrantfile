# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrant configuration version 2
Vagrant.configure(2) do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
  end

  config.vm.box = "ubuntu/xenial64"

  # Synced folder between host machine and guest VM
  config.vm.synced_folder "./", "/home/vagrant/phaser"

  # Forwarded port mappings
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Provisioning the container
  config.vm.provision "shell", inline: <<-SHELL
    sudo add-apt-repository ppa:george-edison55/cmake-3.x
    sudo apt-get -y update
    sudo apt-get -y install g++ git make libboost-atomic-dev libboost-chrono-dev libboost-thread-dev libboost-system-dev libboost-date-time-dev libboost-regex-dev libboost-filesystem-dev libboost-random-dev libboost-serialization-dev libwebsocketpp-dev openssl libssl-dev ninja-build cmake

    # Build the C++ REST SDK ("Casablanca")
    git clone https://github.com/Microsoft/cpprestsdk.git casablanca
    mkdir -p casablanca/Release/build.release
    cd casablanca/Release/build.release
    cmake -G Ninja .. -DCMAKE_BUILD_TYPE=Release
    ninja

    # Build the Azure Storage Client Library for C++
    sudo apt-get -y install g++-4.8 libxml++2.6-dev libxml++2.6-doc uuid-dev
    cd /home/vagrant
    git clone https://github.com/Azure/azure-storage-cpp.git
    cd azure-storage-cpp/Microsoft.WindowsAzure.Storage/
    mkdir build.release
    cd build.release
    CASABLANCA_DIR=/home/vagrant/casablanca CXX=g++-4.8 cmake .. -DCMAKE_BUILD_TYPE=Release
    make

    # Set time zone for Vancouver, Canada
    timedatectl set-timezone Canada/Pacific
    SHELL
  end
