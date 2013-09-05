### The problem

I have a [TP-Link WN722N](http://www.tp-link.com/en/products/details/?model=tl-wn722n) USB Wifi dongle. Linux Mint picked it up during install and seemed like all was good.

Then the other day, noticed that sometimes WiFi would be flaky like hell - all I'd get is the password prompt. Turns out that this is a common problem with USB Wifi.
After a few days, the pattern appeared to be a recurring problem after putting the computer sleep.
The fix is easy - just unload the `ath9k_htc` before suspending.
Edit `/etc/pm/config.d/config` (create if needed):

~~~bash
SUSPEND_MODULES="ath9k_htc"
~~~


