# raspi-ds3231RTC
Installing DS3231 Real Time Clock Module on Raspberry Pi

Source - https://www.raspberrypi.org/forums/viewtopic.php?f=63&t=161133

  Once you have a nice shiny clean raspbian (or raspbian lite) image running (I used raspbian lite exclusively), and you've updated it to latest everything (sudo apt-get update; sudo apt-get upgrade; sudo reboot), you're ready to start.

There are only two edits you need to do:
1. put the below line into the /boot/config.txt file: (edit it with your favourite editor and type the line in - or copy and paste it from here :-) )

dtoverlay=i2c-rtc,ds3231

2. edit the /lib/udev/hwclock-set file (sudo nano /lib/udev/hwclock-set) and "comment out" the following lines ("comment out" means put a # at the beginning of each of the lines, so they become comments and are ignored by the system)

if [ -e /run/systemd/system ] ; then

exit 0

fi

so they become:
>>
#if [ -e /run/systemd/system ] ; then

#exit 0

#fi


...and that's it - that's all you need to do. Shut down your system, connect the rtc module, 
then power up and test with the command:

sudo hwclock -r

this will read the time directly from the rtc module. If a sensible time string comes back, then you're cooking! Fully test by powering off the pi, disconnecting network cable (if you're running wireless, you may need to turn your router off for a bit), plug a screen into the hdmi output of the pi, wait 10 mins, then power it up. Once it's up, using a locally connected keyboard, log in and type "date" - if the time is correct and not 10 mins slow (or however long you had the pi powered down for) then the system has read the time correctly from the rtc module!!

Following on from the above, here's a picture of the modules I used (the connector simply plugs onto the first 5 pins of the pi) - here are some useful hwclock commands to test and play. They all need to be run as the root user, so precede with "sudo" if you're not running as root:

read time directly from rtc module

hwclock -r

update rtc module time from system time (system time should be regularly updated by ntp from the internet if your pi is networked):

hwclock -w

update system time from the rtc module (this should happen on startup):

hwclock -s

and the most fun of all - monitor the "drift" between your system clock and the rtc module:

hwclock -c

the above command will not return to the prompt, it will run continuously, printing the clock drift every 10 seconds, until you interrupt it (with Cntl-C).


PIN INSTALL - Install it on the bottom GPIO pins on the right hand side, away from the USB 

2 4 6 8 10 12 ........      USB PORTS

1 3 5 7 9  11 ........

  Pin#1 3V3
  
  Pin#3 SDA
  
  Pin#5 SCL
  
  Pin#7 GPIO4
  
  Pin#9 GND
  
  https://pinout.xyz

