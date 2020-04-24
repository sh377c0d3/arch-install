## Arch Linux

###### Installation procedure (lvm, mbr):
  1. Use wifi-menu to connect to network
  2. Visit https://www.archlinux.org/mirrorlist/ on another computer, generate mirrorlist
  3. Edit /etc/pacman.d/mirrorlist on the Arch computer and paste the faster servers
  4. Update package indexes: `# pacman -Syyy`
  5. Partition disk, create lvm partition:
       
       `# fdisk /dev/sda`
        
          * o
          * enter
          * n
          * p
          * 1
          * enter
          * enter
          * t
          * 1
          * 8E
          * w
        
  6.  Create lvm:
        * Non-SSD: `# pvcreate /dev/sda1`
        * SSD: `# pvcreate --dataalignment 1m /dev/sda1`
        * `# vgcreate volgroup0 /dev/sda1`
        * `# lvcreate -l 100%FREE volgroup0 -n root`
        * `# modprobe dm_mod`
        * `# vgscan`
        * `# vgchange -ay`

  7. `# mkfs.ext4 /dev/volgroup0/root`
  8. `# mount /dev/volgroup0/root /mnt`
  9. `# pacstrap -i /mnt base base-devel`
  10. `# genfstab -U -p /mnt >> /mnt/etc/fstab`
  11. `# arch-chroot /mnt`
  12. `# pacman -S nano openssh grub-bios linux-headers linux network-manager-applet mkinitcpio networkmanager networkmanager-openvpn wireless_tools wpa_supplicant`
  13. Edit `/etc/mkinitcpio.conf` and add `lvm2` in between `block` and `filesystems`
  14. `# mkinitcpio -p linux`
  15. `# nano /etc/locale.gen (uncomment en_US.UTF-8)`
  16. `# locale-gen`
  17. `# ln -s /usr/share/zoneinfo/Asia/Kolkata /etc/localtime`
  18. `# hwclock --systohc --utc`
  19. `# passwd` (for root)
  20. `# grub-install --target=i386-pc --recheck /dev/sda`
  21. `# cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo`
  22. `# grub-mkconfig -o /boot/grub/grub.cfg`
  23. Create swap file:
        * `# fallocate -l 2G /swapfile`
        * `# chmod 600 /swapfile`
        * `# mkswap /swapfile`
        * `# echo '/swapfile none swap sw 0 0' | tee -a /etc/fstab`

  24. `$ exit`
  25. `# umount /mnt/home`
  26. `# umount /mnt`
  27. `# reboot`
