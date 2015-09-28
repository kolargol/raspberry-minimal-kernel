# raspberry-minimal-kernel
Raspberry PI 2 Minimal Kernel

This repository contains config and binaries for minimal kernel configuration (mostly) for Raspberry PI 2. This kernel is "moduleless" this means that it is not possible to load modules at runtime.
Repository contains:

./bins - pre-build binaries if kernel, ready to install in 2 minutes
./configs - configuration for kernels, so anyone can compile own bins

kernel7.img is _always_ signed by my GPG key (kernel7.img.sig) PublicKey: https://onefellow.com/zbyszek-gpg.asc

How this kernel is differen from oryginal?
 - secure: no modules, includes fstack-protector-strong, removed lot of unessesery modules, soon i hope it will include grsec, and more
 - peformance: includes config towards performance and optimizations, removed a lot of debuging code, optimized for server tasks
 - portability: compiled kernel is a single file ~3MB in size, you can copy it on 2 FDD ;)
 - for detail look at menuconfig
 - it includes most things sysadmin need for servers: iptables, ipsec, TUN/TAP, ipv6 etc...
 - supports bluetooth (BCM2xxx)
 - only ext4

What this kernel do not include?
 - this kernel _is not_ for desktop use
 - it do not include stuff like: sound support, mice, keyboard and crap like that...(who need that this days?)
 - again: no modules
 - no wifi
 - if you want to change it - see Installation 

Installation:
Before you proceed, backup any files overwritten!
1) If you want to install bins:
 - copy bins/KERNEL_VERSION/kernel7.img to /boot/kernel7minimal.img 
 - copy bins/KERNEL_VERSION/overlays to /boot/
 - copy bins/KERNEL_VERSION/bcm2709-rpi-2-b.dtb to /boot/
 - add line: kernel=kernel7minimal.img to yout /boot/config.txt 
reboot and pray ;)

2) Build kernel from configs:
This will not cover building whole thing, just quick roundup how to do it in few steps:
 - clone https://github.com/raspberrypi/linux.git
 - checkout correct branch ex: git checkout rpi-4.1.y
 - copy configs/config-KERNEL_VERSION.txt to place where your source of kernel is and rename it to .config
 - you can make adjustments via make menuconfig and enable needed modules
 - make -j 4 zImage dtbs
 - compilation on RPi2 takes around 30 minutes, so grab a coffe
 - cp arch/arm/boot/dts/*.dtb /boot/
 - cp arch/arm/boot/dts/overlays/*.dtb* /boot/overlays/
 - scripts/mkknlimg arch/arm/boot/zImage /boot/mynewsuperkernel.img
 - edit /boot/config.txt and add line kernel=mynewsuperkernel.img
reboot and pray...



