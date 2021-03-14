# -*- mode: ruby -*-
# vi: set ft=ruby tw=79 formatoptions+=t:
# Vagrantfile for Pi development (creates boxes for 32 and 64 bit kernel
# compiles)

$msg32 = <<MSG
------------------------------------------------------
Vagrant box with ubuntu/xenial32 initialised.
You can now cross-compile the kernel

Enter the following commands to build the sources and Device Tree files:
32-bit configs

For Pi 1, Pi Zero, Pi Zero W, or Compute Module:

cd /usr/src/linux
KERNEL=kernel
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bcmrpi_defconfig

For Pi 2, Pi 3, Pi 3+, or Compute Module 3:

cd /usr/src/linux
KERNEL=kernel7
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bcm2709_defconfig

For Raspberry Pi 4:

cd /usr/src/linux
KERNEL=kernel7l
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bcm2711_defconfig

------------------------------------------------------
MSG

$msg64 = <<MSG
------------------------------------------------------
Vagrant box with ubuntu/xenial64 initialised.
You can now cross-compile the kernel

For Pi 3, Pi 3+ or Compute Module 3:

cd /usr/src/linux
KERNEL=kernel8
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- bcmrpi3_defconfig

For Raspberry Pi 4:

cd /usr/src/linux
KERNEL=kernel8
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- bcm2711_defconfig
MSG


Vagrant.configure("2") do |config|

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  config.vm.provision "shell", inline: <<-SHELL
     apt-get update
     apt -y install git bc bison flex libssl-dev make libc6-dev libncurses5-dev
     # Get the kernel sources
     cd /usr/src
     git clone --depth=1 https://github.com/raspberrypi/linux 2>&1 >/dev/null || \
     (echo 'Error cloning, are the sources already cloned?'; exit 0)
  SHELL

  config.vm.synced_folder "src", "/usr/src"

  config.vm.define "Dev32" do |dev32|
    dev32.vm.box = "ubuntu/xenial32"
    dev32.vm.provider "virtualbox" do |vb32|
      # Name the VM
      vb32.name = "Xenial32 for Pi Development"
    end

    dev32.vm.provision :shell, inline: "apt install -y crossbuild-essential-armhf"
    dev32.vm.post_up_message = $msg32
  end

  config.vm.define "Dev64" do |dev64|
    dev64.vm.box = "ubuntu/xenial64"
    dev64.vm.provider "virtualbox" do |vb64|
      # Name the VM
      vb64.name = "Xenial64 for Pi Development"
    end

    dev64.vm.provision :shell, inline: "apt install -y crossbuild-essential-arm64"
    dev64.vm.post_up_message = $msg64
  end

  config.vm.provider "virtualbox" do |vb|
    # Make sure there are more cores available (as many as nproc reports) or
    # 1 if nproc fails (e.g. on non-Linux hosts)
    vb.cpus = %x{nproc} or "1"

    # Customise the amount of memory on the VM (quarter of the amount on a
    # Linux PC) or 1024 if the command can't be executed (e.g. if running on a
    # non-Linux host):
    vb.memory = %x{awk '$1 == "MemTotal:" {printf "%d",$2 / 4096}' \
                /proc/meminfo} or "1024"
  end

end
