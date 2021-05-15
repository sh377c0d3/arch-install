
###### Post installation steps:
  1. Fix GNOME app issues: `# localectl set-locale LANG="en_US.UTF-8"`
  2. Add to `fstab`:
	   `tmpfs   /tmp         tmpfs   nodev,nosuid,size=2G          0  0`
  3. If ssd, add `discard` to `fstab`. Example:
	   `UUID=<UUID>	/         	ext4      	defaults,noatime,discard	0 2`


# **** Add User ****

Uncomment `%wheel ALL =(ALL) ALL` from `/etc/sudoers`

Then Run:

`user@host:~# useradd -m -g users -G wheel,storage,power -s /bin/bash superuser`

# Enable Network Manager

Run:

`systemctl enable NetworkManager`

## Other Utility 

sudo pacman -S freetype2 terminus-font ttf-bitstream-vera ttf-dejavu ttf-droid ttf-fira-mono ttf-fira-sans ttf-freefont ttf-inconsolata ttf-liberation ttf-linux-libertine ttf-ubuntu-font-family xorg-xfontsel alsa-firmware alsa-utils ffmpeg flac gst-libav gst-plugins-base gst-plugins-good gstreamer lame libdvdcss libdvdnav libdvdread libmpeg2 libtheora libvorbis x264 x265 bzip2 p7zip unrar tar rsync

# Enjoy Arch Linux
