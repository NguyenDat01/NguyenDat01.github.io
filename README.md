# NguyenDat01.github.io

Pre-Installation Process
Looked up ISO list from the list of mirror sites of HTTP downloads and chose the mit.edu one.
Verified signature of the ISO from the ISO using the SHA256 and b2 sums
Opened virtual box and made a new VM![[Pasted image 20251029004707.png]]
In VirtualBox I had to enable UEFI by going to settings and clicking on expert, then going to system and check marking the UEFI option
Started the VM and it opened up the console signed in as root![[Pasted image 20251029005557.png]]
Ran cat /sys/firmware/efi/fw_platform_size to verify boot mode and 64 was outputting to verify that the VM booted in UEFI
Verified internet connection by running ping ping.archlinux.org and was given an output to verify internet connection
ran timedatectl to verify that the system clock is synchronized. ![[Pasted image 20251029161444.png]]
Made a snapshot right before beginning partitioning because i don't really understand what I'm doing and feel like I will mess up. I eventually found out that we have to make a partition inside /dev/sda
Ran fdisk /dev/sda to get into that disk to modify the partitions.
g to make an empty GPT table
n to make a new partition
Made a mistake when making this EFI partition: entered "1" for partition number, then " ", "+500" and I didn't specify how big it was so it made it into a unit that wasn't in MB. I redid it and entered 1, , +500M
Have to change the type of the partition because it should be EFI but is classified as a Linux filesystem. typed "t", and had to look through the list for EFI which is 1.
Make another partition for the root using the rest of the storage available. Commands that I typed in: "n" " " " " blank commands to use the rest of the storage.
What the partition looks like![[Pasted image 20251029175237.png]]
Format the partitions by mkfs.ext4 /dev/sda2 and mkfs.fat -F 32 /dev/sda1
Mount files by using mount /dev/sda2 /mnt for the root file
make a new directory for a new mount point using mkdir /mnt/boot and then `mount /dev/sda1 /mnt/boot
Installation
Run pacstrap -K /mnt base linux linux-firmware to install basic hardware for Linux.
Run archinstall and went to disk configuration to select the partitioning that I just did and set it to the /mnt directory
went to authentication to set password for root as "admin"
went to user account in authentication and selected add a user with the user dat, and password pizza01. Make another account for codi with password teacher01
After that I clicked install with desired configuration
it took me a little bit to realize that after i rebooted the system, it did not boot into the arch but that I was still in the iso. To boot into the arch linux, I had to close it and go to settings in Virtual Box, then to storage and delete the iso so that it will boot from the HDD
I realized that i should've not booted into arch linux yet so i have to go back, but good thing I made a snapshot right after i installed the arch linux!
this time after installing, i chose the option "chroot into installation for post-installation configuration" which is still the iso command line but before i rebot into the arch linux system
Before rebooting into the new system, i ran genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt to directly interact with the new system as if i booted into it.
installing DE(KDE Plasma)
Followed a tutorial on how to install KDE Plasma

pacman -S plasma xorg kde-applications to download desktop environment.
systemctl enable sddm.service to enable to DE
systemctl enable NetworkManager.service for network services
Went back to virtual box and deleted the ISO from the HDD and booted up the vm![[Pasted image 20251030002747.png]]![[Pasted image 20251030002940.png]]
Configuring the system
sudo ln -sf /usr/share/zoneinfo/Region/City /etc/localtime and sudo hwclock --systohc to update time
i switched users to root then, ran locale-gen and since i have to edit /etc/locale.conf, so i need to install a text editor, nano using pacman -S nano
nano /etc/locale.gen to make sure "en_US.UTF-8" is uncommented
nano /etc/vconsole.conf to make sure keymap is us which is the keyboard layout.
nano /etc/hostname to make sure our system has an identifiable name which is archlinux which should be fine.
Installing a boot loader
Install grub and efibootmgr by pacman -S grub efibootmger
remount EFI by mkdir /mnt/boot then mount dev/sda1 /mnt/boot
grub-install --target=x86_64-efi --efi-directory=/mnt/boot --bootloader-id=GRUB installs grub to the disk
grub-mkconfig -o /boot/grub/grub.cfg makes the main configuration file
Installing extra packages
Installation of another shell such as zsh by using pacman -S zsh
installation of ssh by running pacman -S openssh and then systemctl enable sshd
added color coding by running nano ~/.bashrc and then modifying the PS1 variable to PS1='\[\e[1;32m\]\u@\h \[\e[1;34m\]\w \$\[\e[0m\] ' and the enableing by running source ~/.bashrc
installation of aliases
to permanently add an alias, we have to nano ~/.bashrc and add in our aliases such as ![[Pasted image 20251102193347.png]]
