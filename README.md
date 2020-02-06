# opensuse-on-pinebookpro-gnome
openSUSE on pinebookpro with Gnome on Wayland. 

grab the image here:

https://sourceforge.net/projects/opensuse-on-pinebookpro/files/Rel_1/

Instructions: You must have more than 16G of space to write this image! Preferably 32G or larger

To write image to sdcard from a linux pc: xzcat opensuse-tumbleweed-pinebookpro-
gnome-1.0.img.xz | dd bs=4M of=/dev/mmcblkX iflag=fullblock oflag=direct status=progress; sync

Because of the level of compression I used, writing this image to internal disk from pinebookpro via an os running on a sdcard to internal memory requires you unxz --threads=(max number of threads your pc processor has) -6e fedora-pinebookpro-gnome-0.8.img.xz

from a linux pc, then you can copy the disk image to the os running on pinebookpro via sdcard and enter: dd if=opensuse-tumbleweed-pinebookpro-gnome-1.0.img of=/dev/mmcblkX oflag=sync status=progress bs=32M


When you first boot, login as user "tux". The password is "susepassword".
The root password is "linux".

You can change your username/password with the YaST Users tool from the applications overlay/menu
Be sure to change your time and date settings with YaST Time and Date. After that, reboot and you should be good to go!

Then, you have to resize your disk to fill the remaining space. This could be done by entering:

# sudo cfdisk /dev/mmcblkX

then resize your last partition. Make sure to hit write and then exit. Then enter:

# sudo resize2fs /dev/mmcblkXp6

and voilia! Disk resized.

# Caveats

This image boots up slowly. Especially on the first boot, give it 3-4 minutes. I'm going to play around with the kernel configuration to speed this up. I've had the same issue with my opensuse-xfce and Fedora images as well. Once you've booted up and logged in, It's smooth sailing. Also, thanks to the Manjaro team, suspend and resume is now working, so you shouldn't have to powerdown and poweron all that much! Thanks again guys!

______________________________________________________________________________________________________________________________________

I'm going to be adding kernel rpm's on this repo. One will be labeled "apparmor" and the other "selinux". Both could be used with this, or my fedora images. Depends on your preferences.

Apparmor is mostly in permissive mode, so to enable everything, just run:

# sudo aa-enforce /etc/apparmor.d/*

and then everything will go into enforcing mode.

Because of media codec issues with openSUSE on the aarch64 platform, chromium is running in a Debian systemd-nspawn container. You'll be prompted to enter the root password either once or twice to start up chromium.

If you want to change your sudo password to your own, rather than entering the root password, enter:

# sudo usermod -aG wheel,trusted,audio,video yourusername

then, switch to root user with 

# su -

Then,

# visudo

Comment lines 68 and 69, then uncomment line 81. Reboot or login and logout again, and your sudo password will be switched to your user password, rather than the root users password.

Don't bother running Gnome on Xorg, it runs like garbage. Oddly enough, Fedora on pinebook pro has the inverse problem. Who knows?
Anyway, hope you enjoy it!

Have a lot of fun...
