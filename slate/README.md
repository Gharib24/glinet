# Slate (GL-AR750S-Ext)  
The slate does not have intelligent LEDs like the Beryl so it's either on or off, no dimming, period.  

The commands below are based on firmware version 3.203.  

## Overview  
To turn off the LEDs run the following command:  
```bash
# 0 disables brightness
find /sys/devices/platform/leds/leds \! -path '*usb*' -name brightness -exec sh -c 'echo 0 > {}' \;
```

To turn the LEDs back on: 
```bash
# 255 enables brightness
find /sys/devices/platform/leds/leds \! -path '*usb*' -name brightness -exec sh -c 'echo 255 > {}' \;
```  

## Installation  
SSH into the Slate device.  
1.  Run `date` to validate which time zone, it should default to GMT/UTC.  
2.  Add a cronjob for this to work, either via the LuCi Scheduled Tasks or CLI crontab.  Example of doing it in the CLI:  
   ```bash
   # Edit the crontab
   crontab -e
   # Type a single i on the keyboard to go into Insert mode
   i
   # After making the changes in step 3, write and quit
   :wq
   ```
3.  Insert / Paste the following commands into the crontab.  Adjust the times as needed.  
   ```bash
   # Turn LEDs off for sleep (GMT)
   0 12 * * * find /sys/devices/platform/leds/leds \! -path '*usb*' -name brightness -exec sh -c 'echo 0 > {}' \;
   # Turn LEDs on (GMT)
   0 22 * * * find /sys/devices/platform/leds/leds \! -path '*usb*' -name brightness -exec sh -c 'echo 255 > {}' \;
   ```
This will turn the LEDs off at 8 PM (GMT+8 timezone).  Then it will turn them back on the next morning at 6 AM (GMT+8 timezone).  

4.  Reload the cron service.
   ```bash
   service cron reload
   ```
