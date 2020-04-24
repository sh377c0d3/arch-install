## Arch Linux

###### Installation procedure (lvm, mbr):
  1. Use wifi-menu to connect to network
  2. Visit https://www.archlinux.org/mirrorlist/ on another computer, generate mirrorlist
  3. Edit /etc/pacman.d/mirrorlist on the Arch computer and paste the faster servers
  4. Update package indexes: `# pacman -Syyy`
  5. Create boot partition:

      `# fdisk /dev/sda`

         * o
         * n
         * p
         * 1
         * enter
         * +500M
         * t
         * a
         * n
         * p
         * 2
         * enter
         * enter
         * t
         * 2
         * 8E
         * w 

  6. `# mkfs.ext2 /dev/sda1`
  7.  Set up encryption
        * `# cryptsetup luksFormat /dev/sda2`
        * `# cryptsetup open --type luks /dev/sda2 lvm` 
        
  8.  Set up lvm:
        * Non-SSD: `# pvcreate /dev/mapper/lvm`
        * SSD: `# pvcreate --dataalignment 1m /dev/mapper/lvm`
        * `# vgcreate volgroup0 /dev/mapper/lvm`
        * `# lvcreate -l 100%FREE volgroup0 -n root`
        * `# modprobe dm_mod`
        * `# vgscan`
        * `# vgchange -ay`

  9. `# mkfs.ext4 /dev/volgroup0/root`
  10. `# mount /dev/volgroup0/root /mnt`
  11. `# mkdir /mnt/boot`
  12. `# mount /dev/sda1 /mnt/boot`
  13. `# pacstrap -i /mnt base base-devel`
  14. `# genfstab -U -p /mnt >> /mnt/etc/fstab`
  15. `# arch-chroot /mnt`
  16. `# pacman -S nano grub-bios os-prober linux linux-headers mkinitcpio lvm2 network-manager-applet networkmanager networkmanager-openvpn wireless_tools wpa_supplicant`
  17. Edit `/etc/mkinitcpio.conf` and add `encrypt` and `lvm2` in between `block` and `filesystems`
  18. `# mkinitcpio -p linux`
  19. `# nano /etc/locale.gen (uncomment en_US.UTF-8)`
  20. `# locale-gen`
  21. `# ln -s /usr/share/zoneinfo/Asia/Kolkata /etc/localtime`
  22. `# hwclock --systohc --utc`
  23. `# passwd` (for root)
  24. Edit `/etc/default/grub`:
        add `cryptdevice=<PARTUUID>:volgroup0` to the `GRUB_CMDLINE_LINUX_DEFAULT` line
            If using standard device naming, the option will look like this: `cryptdevice=/dev/sda2:volgroup0`
  25. `# grub-install --target=i386-pc --recheck /dev/sda`
  26. `# cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo`
  27. `# grub-mkconfig -o /boot/grub/grub.cfg`
  28. Create swap file:
        * `# fallocate -l 2G /swapfile`
        * `# chmod 600 /swapfile`
        * `# mkswap /swapfile`
        * `# echo '/swapfile none swap sw 0 0' | tee -a /etc/fstab`

  29. `$ exit`
  30. `# umount /mnt/home`
  31. `# umount /mnt`
  32. `# reboot`
