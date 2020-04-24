## Arch Linux

###### Installation procedure (lvm with no encryption):
  1. Use wifi-menu to connect to network
  2. Visit https://www.archlinux.org/mirrorlist/ on another computer, generate mirrorlist
  3. Edit /etc/pacman.d/mirrorlist on the Arch computer and paste the faster servers
  4. Update package indexes: `# pacman -Syyy`
  5. Create efi partition:

       `# fdisk /dev/sda`

          * g (to create an empty GPT partition table)
          * n
          * 1
          * enter
          * +300M
          * t
          * 1 (For EFI)
          * n
          * 2
          * enter
          * enter
          * t
          * 2
          * 31
          * w

  6. `# mkfs.fat -F32 /dev/sda1`
  7. Set up lvm:
        * Non-SSD: `# pvcreate /dev/sda2`
        * SSD: `# pvcreate --dataalignment 1m /dev/sda2`
        * `# vgcreate volgroup0 /dev/sda2`
        * `# lvcreate -l 100%FREE volgroup0 -n root`
        * `# modprobe dm_mod`
        * `# vgscan`
        * `# vgchange -ay`
        
  8. `# mkfs.ext4 /dev/volgroup0/root`
  9. `# mount /dev/volgroup0/lv_root /mnt`
  10. `# pacstrap -i /mnt base base-devel`
  11. `# genfstab -U -p /mnt >> /mnt/etc/fstab`
  12. `# arch-chroot /mnt`
  13. `# pacman -S nano grub efibootmgr dosfstools os-prober mtools linux-headers linux mkinitcpio lvm2 network-manager-applet networkmanager networkmanager-openvpn wireless_tools wpa_supplicant`
  14. Edit `/etc/mkinitcpio.conf` and add `lvm2` in between `block` and `filesystems`
  15. `# mkinitcpio -p linux`
  16. `# nano /etc/locale.gen` (uncomment en_US.UTF-8)
  17. `# locale-gen`
  18. `# ln -s /usr/share/zoneinfo/Asia/Kolkata /etc/localtime`
  19. `# hwclock --systohc --utc`
  20. `# passwd` (for setting root password)
  21. `# mkdir /boot/EFI`
  22. `# mount /dev/sda1 /boot/EFI`
  23. `# grub-install --target=x86_64-efi  --bootloader-id=grub_uefi --recheck /dev/sda`
  24. `# cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo`
  25. `# grub-mkconfig -o /boot/grub/grub.cfg`
  26. Create swap file:
        * `# fallocate -l 2G /swapfile`
        * `# chmod 600 /swapfile`
        * `# mkswap /swapfile`
        * `# echo '/swapfile none swap sw 0 0' | tee -a /etc/fstab`
  27. `$ exit`
  28. `# umount -a
  29. `# reboot`
