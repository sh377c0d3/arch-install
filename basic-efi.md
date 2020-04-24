## Arch Linux

###### General Installation procedure (standard install on EFI):
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
          * +400M
          * t
          * 1 (For EFI)
          * n
          * 2
          * enter
          * enter
          * w

  6. `# mkfs.fat -F32 /dev/sda1`
  7. `# mkfs.ext4 /dev/sda2`
  9. `# mount /dev/sda2 /mnt`
  11. `# mkdir /boot/EFI`
  12. `# mount /dev/sda1 /boot/EFI`
  13. `# pacstrap -i /mnt base base-devel`
  14. `# genfstab -U -p /mnt >> /mnt/etc/fstab`
  15. `# arch-chroot /mnt`
  16. `# pacman -S nano grub efibootmgr dosfstools os-prober mtools linux-headers linux network-manager-applet networkmanager networkmanager-openvpn wireless_tools wpa_supplicant`
  17. `# nano /etc/locale.gen` (uncomment en_US.UTF-8)
  18. `# locale-gen`
  19. `# ln -s /usr/share/zoneinfo/Asia/Kolkata /etc/localtime`
  20. `# hwclock --systohc --utc`
  21. `# passwd` (for setting root password)
  22. `# grub-install --target=x86_64-efi  --bootloader-id=grub_uefi --recheck /dev/sda`
  23. `# cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo`
  24. `# grub-mkconfig -o /boot/grub/grub.cfg`
  25. Create swap file:
        * `# fallocate -l 2G /swapfile`
        * `# chmod 600 /swapfile`
        * `# mkswap /swapfile`
        * `# echo '/swapfile none swap sw 0 0' | tee -a /etc/fstab`
  26. `$ exit`
  27. `# umount -a`
  28. `# reboot`
