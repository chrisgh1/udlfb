# udlfb
su

apt-get update

apt-get install linux-headers-$(uname -r)

cd /usr/src

apt-get install git

git clone https://github.com/chrisgh1/udlfb.git
mv udlfb udlfb-0.5

cd udlfb-0.5

apt-get install dkms

dkms add -m udlfb -v 0.5

ln -s /usr/src/linux-headers-$(uname -r) /lib/modules/$(uname -r)/build

dkms build -m udlfb -v 0.5
dkms install -m udlfb -v 0.5

nano /etc/modprobe.d/blacklist-framebuffer.conf
comment out blacklist udlfb

Loading module at startup

add udlfb //fb_defio=1 to /etc/modules

shutdown -r now

when odroid c2 comes back up you should get a green screen on the mimo

add the following to /etc/X11/xorg.conf below the current device
comment out the current device to disable HDMI and enable MiMo

Section "Device" 
  Identifier "uga" 
  driver "fbdev" 
  Option "fbdev" "/dev/fb2" 
  Option "ShadowFB" "off"
# This option rotates the screen but need to figure out how to rotate touch
#  Option "rotate" "CW"
EndSection 

shutdown -r now

add autologin-user=odroid to /usr/share/lightdm/lightdm.conf.d/60-lightdm-gtk-greeter.conf
