# Vagrant Pi Dev
This is a Vagrantconfig for spinning up an Ubuntu development environment for
cross compiling the Linux kernel (or any other cross compiling need)

## Usage
`vagrant up` spins up two boxes, one 32-bit and one 64-bit.  
Specify the box you want if you just want to spin up one, e.g. `vagrant up Dev32`
for 32-bit.

There are two directories shared into the Virtualbox VM; the current directory
and the `src` directory in this directory. The first one is mapped to `/vagrant`
and the other one to `/usr/src` where the Raspberry Pi Linux kernel source is
cloned to.
