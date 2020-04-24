## Arch Linux

###### Installation procedure (encrypted lvm on EFI):
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
          * +400M
          * n
          * 3
          * enter
          * enter
          * t
          * 3
          * 31
          * w

  6. `# mkfs.fat -F32 /dev/sda1`
  7. `# mkfs.ext2 /dev/sda2`
  8. Set up encryption
        * `# cryptsetup luksFormat /dev/sda3`
        * `# cryptsetup open --type luks /dev/sda3 lvm`
  9. Set up lvm:
         * Non-SSD: `# pvcreate /dev/mapper/lvm`
        * SSD: `# pvcreate --dataalignment 1m /dev/mapper/lvm`
        * `# vgcreate volgroup0 /dev/mapper/lvm`
        * `# lvcreate -l 100%FREE volgroup0 -n root`
        * `# modprobe dm_mod`
        * `# vgscan`
        * `# vgchange -ay`
  10. `# mkfs.ext4 /dev/volgroup0/root`
  11. `# mount /dev/volgroup0/root /mnt`
  13. `# mkdir /mnt/boot`
  13. `# mount /dev/sda2 /mnt/boot`
  14. `# mkdir /boot/EFI`
  15. `# mount /dev/sda1 /boot/EFI`
  16. `# pacstrap -i /mnt base base-devel`
  17. `# genfstab -U -p /mnt >> /mnt/etc/fstab`
  18. `# arch-chroot /mnt`
  19. `# pacman -S nano grub efibootmgr dosfstools os-prober mtools linux-headers linux mkinitcpio lvm2 network-manager-applet networkmanager networkmanager-openvpn wireless_tools wpa_supplicant`
  20. Edit `/etc/mkinitcpio.conf` then add `encrypt` and `lvm2` in between `block` and `filesystems`
  21. `# mkinitcpio -p linux`
  22. `# nano /etc/locale.gen` (uncomment en_US.UTF-8)
  23. `# locale-gen`
  24. `# ln -s /usr/share/zoneinfo/Asia/Kolkata /etc/localtime`
  25. `# hwclock --systohc --utc`
  26. `# passwd` (for setting root password)
  27. Edit `/etc/default/grub`:
        add `cryptdevice=<PARTUUID>:volgroup0` to the `GRUB_CMDLINE_LINUX_DEFAULT` line
            If using standard device naming, the option will look like this: `cryptdevice=/dev/sda3:volgroup0`
  28. `# mkdir /boot/EFI`
  29. `# mount /dev/sda1 /boot/EFI`
  30. `# grub-install --target=x86_64-efi  --bootloader-id=grub_uefi --recheck /dev/sda`
  31. `# cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo`
  32. `# grub-mkconfig -o /boot/grub/grub.cfg`
  33. Create swap file:
        * `# fallocate -l 2G /swapfile`
        * `# chmod 600 /swapfile`
        * `# mkswap /swapfile`
        * `# echo '/swapfile none swap sw 0 0' | tee -a /etc/fstab`
  34. `$ exit`
  35. `# umount -a`
  36. `# reboot`
