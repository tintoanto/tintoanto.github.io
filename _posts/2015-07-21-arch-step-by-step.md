---
layout: post
title: Arch Step-by-Step
---

## 1. Partitioning the Hard Drive

*  Identify the disk

		fdisk -l

*  Partition the disk

		cfdisk /dev/sda

*  Format the ext4 partition

		mkfs.ext4 /dev/sda2

*  Mount the root drive

		mount /dev/sda2 /mnt

*  Create swap file on the swap partition

		mkswap /dev/sda1			

*  Turn on swap utilisation

		swapon /dev/sda1

## 2. Installing the System

* Install the base system to the root drive(currently mounted to /mnt)
	+ Downloads approx. 250MB

			pacstrap /mnt base base-devel


## 3. Configuring the Installation

* Set a few things up
	* Set the root password
		
			arch-chroot /mnt
			passwd 
			****

	* Set locale

			nano /etc/local.gen
			\/\/Uncomment the selected locale and save

	* create the locales
		
			locale-gen

	* Set Time zone
		* Identify Time zone
		
				cd /usr/share/zoneinfo
				ls
				cd <Country>
				ls

		* Set the Time zone
				
				ln -s /usr/share/zoneinfo/<Country>/<Region> /etc/localtime

	* Set hostname
			echo <hostname> > /etc/hostname

	* Install Bootloader
		* Download Grub
		
				pacman -S grub-bios

		* Install grub

				grub-install sda

	* Generate init file
			
			mkinitcpio -p linux

	* Create grub configuration file
	
			grub-mkconfig -o /boot/grub/grub.cfg

	* Exit chroot session
	
			exit

* Generate fstab file

		genfstab /mnt >> /mnt/etc/fstab

* Unmount root

		umount /mnt

* Reboot the system

		reboot

## 4. Configuring the Network
* Identify the Network Interface and the Kernel driver used

		lscpi -v | less

* Get the Interface name

		dmesg | grep e1000

* Turn the interface on

		ip link set eno16777736 up

* Turn on dhcp
	
		dhcpcd eno16777736

## 5. Installing GUI
* Install Gnome Desktop

		pacman -S gnome

* Install Xorg-xinit

		pacman -S xorg-xinit

## 6. Start the GUI

		startx