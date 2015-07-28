# Arch-Linux-rMBP
A simple guide to help you install Arch on your Retina Mac Book Pro
I looked at Jorisvandijk.com for help so credit to him/her.

1) Create a Usb/Cd install disk with Arch <br>
2) Boot Arch using the option key and selecting Arch Efi. Once it's finished
use loadkeys to set your keyboard if you have a US keymap you probably wont have to do anything <br> <br>
3) You need Internet so I usually use my android phone for tethering. <br>
4) Do dhcpcd and ping google.com to see if you have inernet access. If everything works go to step 5<br>
5) Find what's the name of your ssd most of the time it's /dev/sda. Try fdisk -l to list HDD's.
Replace sda by your ssd's name in the following steps<br>
6) Type mkfs.ext4 /dev/sda <br>
7) Type gdisk /dev/sda <br>
8) Type n to create new partition, It will ask you for first sector select Default. last sector type 1024M. Then type of partition =  EF00. This is your 
boot partition<br>
9) Create a new partition with n. first sector Default. last sector 15G ajust it to your liking. type of partition = 8300. 
This is your root partition  <br>
10)Last but not least create a new partition the same way with n and choose default everywhere by hitting 
enter repetitively like a monkey. This will create the home partition with the remaining space on your ssd <br>
11) Type p to see what your partition table looks like. You should see 3 partitions one with the EFI tag and two linux file
systems. If everything is OK hit w to write the changes. <br> <br>
12) Now format your partitions <br>
mkfs.ext4 /dev/sda2 <br>
mkfs.ext4 /dev/sda3 <br>
mkfs.fat -F32 /dev/sda1 <br> <br>
13) Mount filesystems <br>
mount /dev/sda2 /mnt <br>
mkdir /mnt/boot <br>
mkdir /mnt/home <br>
mount /dev/sda1 /mnt/boot <br>
mount /dev/sda3 /mnt/home <br><br>
14) Install system <br>
pacstrap /mnt base base-devel <br><br>
15) Set up fstab<br>
genfstab -U -p /mnt >> /mnt/etc/fstab <br><br>
16) Configure System <br>
arch-chroot /mnt <br> <br>
17) Choose locale by removing hashtag <br>
nano /etc/locale.gen  save file with ctrl-x. Hit Y to save then type locale-gen to generate locales <br><br>
18) If you choose another language for locales replace this with your language <br><br>
echo LANG=en_US.UTF-8 > /etc/locale.conf<br>
export LANG=en_US.UTF-8<br><br>
19) This is where you give a name to your computer better be cool. <br>
echo nameofyourpc > /etc/hostname <br><br>
20) Install Bootloader<br> 
pacman -S gummiboot <br>
gummiboot install <br>
nano /boot/loader/entries/arch.conf <br>
put in file :<br><br>
title Arch Linux<br>
linux /vmlinuz-linux<br>
initrd /initramfs-linux.img<br>
options root=/dev/sda2 rw<br><br>
Save with ctrl-x and Y<br><br>
21)Set root password with passwd<br><br>
22) Create your username and set passwd and add to suoders file (replace yourusername with your actual username in case your slow)<br><br>
useradd -m -g users -G wheel -s /bin/bash yourusername  <br>
passwd yourusername<br>
echo 'yourusername ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers <br><br>
23) Now we're finished so reboot your pc  type all of this<br>
exit <br>
umount -R /mntn <br>
reboot <br>






