# Beryl (GL-MT1300)  
https://www.gl-inet.com/products/gl-mt1300/  

These two script modifications were based on release 3.203.  

## Overview  
`mt1300_led` - Script that controls the LED itself via `i2cset` commands.  I've added a brightness section (and math) instead of leaving the hard coded 100% brightness.  
* Modify `brightness=10` to be a desired integer between 1-100.  
This uses quick maths to get a pretty close percentage of the total brightness.  100% is insanely bright, and 5-10% is about ideal.  


`gl_mt1300_led_daemon` - Script that runs on an infinite while loop to set the LED based on current running state.  I've two tests that check the current time (for LED off/Sleep) and if a VPN (OpenVPN/Wireguard) exists.  
* Modify the sleep section, in whole integer hours (based on UTC) for when you want the LEDs to flip off.  
  * `if [ "$currenttime" -ge "12" ] && [ "$currenttime" -lt "22" ]`
* Modify the VPN section to have a visual indicator when a tunnel exists.  This is set to white breath.  Other options for this are listed at the bottom of `mt1300_led`  

## Installation  
1.  SCP the files onto the device  
2.  Change the permissions to 775 via chmod
  ```bash
  chmod 775 mt1300_led
  chmod 775 gl_mt1300_led_daemon
  ```
3.  Put the files in `/usr/bin`  
  ```bash
  cp mt1300_led /usr/bin/mt1300_led
  cp gl_mt1300_led_daemon /usr/bin/gl_mt1300_led_daemon
  ```
4.  Restart the led service  
  ```bash
  /etc/init.d/led restart
  ```
