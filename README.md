# Raspberry Pi Install for 5inch Touchscreen
Raspberry Pi Touchscreen Display Driver Instructions

LCD-Show doesn't seem to work very well, so i've taken raspi-lcd script along with other curated instructions and trial and errors and have now managed to get this touchscreen to work.


## Manual Instructions

sudo apt-get install xserver-xorg-input-evdev

Append the below to /boot/config.txt
```
max_usb_current=1
hdmi_group=2
hdmi_mode=87
hdmi_cvt=800 480 60 6 0 0 0
hdmi_drive=1
disable_overscan=0
hdmi_force_hotplug=1
config_hdmi_boost=7
dtoverlay=ads7846,cs=1,penirq=25,penirq_pull=2,speed=50000,keep_vref_on=0,pmax=255,xohms=150
display_rotate=0
```
sudo cp -rf /usr/share/X11/xorg.conf.d/10-evdev.conf /usr/share/X11/xorg.conf.d/45-evdev.conf

Add to 45-edev.conf
```
Section "InputClass"
	Identifier "evdev touchscreen catchall"
	MatchIsTouchscreen "on"
	MatchDevicePath "/dev/input/event*"
	Driver "evdev"
EndSection
```

Edit or create
/ect/X11/xorg.conf.d/99-calibration.conf
```
Section "InputClass"
  Identifier  "calibration"
  MatchProduct  "ADS7846 Touchscreen"
  Option  "Calibration" "280 3905 288 3910"
  Option  "SwapAxes"  "0"
EndSection
```

