
###### Post installation steps:
  1. Fix GNOME app issues: `# localectl set-locale LANG="en_US.UTF-8"`
  2. Add to `fstab`:
	   `tmpfs   /tmp         tmpfs   nodev,nosuid,size=2G          0  0`
  3. If ssd, add `discard` to `fstab`. Example:
	   `UUID=<UUID>	/         	ext4      	defaults,noatime,discard	0 2`


# **** Add User ****

Uncomment `%wheel ALL =(ALL) ALL` from /etc/sudoers

Then Run:

`user@host:~# useradd -m -g users -G wheel,storage,power -s /bin/bash superuser`

# Enable Network Manager

Run:

`systemctl enable NetworkManager.service`

# Enjoy Arch Linux
