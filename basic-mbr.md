## Arch Linux

### Installation procedures
###### Installation procedure (basic install):
  1. Use wifi-menu to connect to network
  2. Visit https://www.archlinux.org/mirrorlist/ on another computer, generate mirrorlist
  3. Edit /etc/pacman.d/mirrorlist on the Arch computer and paste the faster servers
  4. Update package indexes: `# pacman -Syyy`
  5. Create root partition:

       `# fdisk /dev/sda`

          * o (to create an empty DOS partition table)
          * n
          * 1
          * enter
          * +50G (Storage for root)
          * w

  7. `# mkfs.ext4 /dev/sda1`
  8. `# mount /dev/sda1 /mnt`
  9. `# pacstrap -i /mnt base base-devel`
  10. `# genfstab -U -p /mnt >> /mnt/etc/fstab`
  11. `# arch-chroot /mnt`
  12. `# pacman -S nano grub-bios network-manager-applet networkmanager networkmanager-openvpn wireless_tools wpa_supplicant linux-headers linux mkinitcpio`
  13. `# mkinitcpio -p linux`
  15. `# nano /etc/locale.gen` (uncomment en_US.UTF-8)
  16. `# locale-gen`
  17. `# ln -s /usr/share/zoneinfo/Asia/Kolkata /etc/localtime`
  18. `# hwclock --systohc --utc`
  19. `# passwd` (for setting root password)
  20. `# grub-install --target=i386-pc --recheck /dev/sda`
  21. `# cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo`
  22. `# grub-mkconfig -o /boot/grub/grub.cfg`
  23. Create swap file:
        * `# fallocate -l 2G /swapfile`
        * `# chmod 600 /swapfile`
        * `# mkswap /swapfile`
        * `# echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab`
  24. `# exit`
  25. `# umount -a`
  26. `# reboot`
