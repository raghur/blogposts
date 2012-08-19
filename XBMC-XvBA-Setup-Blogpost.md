"=========== Meta ============
"StrID : 477
"Title : Media center setup - XBMC-XVBA
"Slug  : Media-center-setup-XBMC-XVBA
"Cats  : mediacenter
"Tags  : Linux, Ubuntu, xbmc, tips
"=============================
"EditType   : post
"EditFormat : Markdown
"TextAttach : /home/wpcom/public_html/wp-content/blogs.dir/8fe/814317/files/2012/07/wpid-homewpcompublic_htmlwp-contentblogs-dir8fe814317files201207wpid-homewpcompublic_htmlwp-contentblogs-dir8fe814317files201207wpid-homewpcompublic_htmlwp-contentblogs-dir8fe814317files201207vimpress.odt
"========== Content ==========
I finally got my nettop - [AMD E-350 based barebones](http://www.newegg.com/Product/Product.aspx?Item=N82E16856119050) system. Installed [ 4G of RAM ](http://www.newegg.com/Product/Product.aspx?Item=N82E16820231341) and the plan was to set it up with XBMCBuntu or XBMC-XvBA. Instead of installing the XBMC-XvBA version directly, I figured that I could start with XBMCBuntu, see how it does and then if necessary move to the XvBA enabled builds.

I don't have a hard drive for the nettop - the plan was to have the system run off a 8Gig pen drive.

### Basic Installation - XBMCBuntu

#### What you need

1. The nettop with RAM installed.
2. 2 USB pendrives - One for installation (2GB) and another which is going to act as your HDD (8G)

#### Steps

1. Download [ UNetBootin for windows ](http://unetbootin.sourceforge.net/) and the [ XBMCBuntu iso image ](http://mirrors.xbmc.org/releases/XBMCbuntu/xbmcbuntu-11.0.iso)
2. Create a Live USB using UNetBootin: Once you have UNetBootin installed, stick in a flash drive in the usb, start UNetBootin and selec the XBMCBuntu iso image as the source distribution iso and the flash drive as the destination.
3. Boot the nettop using the USB drive: You might have to play around with boot devices and priorities in the BIOS settings to get it to boot from the USB drive. To keep things simple, stick the pendrive into one of the USB2 ports (avoid the USB3)
4. ON the UNetBootin boot menu, you can just try out XBMCBuntu live image. I did so and things seemed to work well enough for me to do the full install to another USB drive plugged into the system. Note that if you're not able to find the target drive, then just reboot with both the USB drives plugged in - sometimes, newly inserted devices aren't detected.
5. Install, go through the menus and wait for it to complete. 
6. As you go through the menus, keep in mind to choose a custom partitioning scheme. In my case, I had 4G of RAM and there's no sense in having a swap drive on the pen drive. If you plan on having hibernation support, then use a 2G swap partition (50% of RAM) - else you can skip the swap altogether.
6. Once done, pull out the installation pen drive and reboot. You should be able to reboot off the USB pendrive that you installed into. The installation pendrive is pretty much done - you won't need it any longer.

####  XBMCBuntu
At this point, I had XBMCBuntu up and running however, there were a few problems:

1. On idle, CPU utilization was very high (~ 60 - 70%) and the unit was running hot. 
1. Display resolution proved troublesome - my LCD's native resolution is 1366x768 but that wasn't available over HDMI. 
1. I was able to get 1360x768 on DVI/D-Sub - but that meant using a separate cable for audio out.

Of these, the high CPU utilization was the biggest worry - so there's a few steps available to try

1. Within XBMC - set sync to display refresh - always.
1. Turn off RSS feeds
1. Tweaks `.xbmc/userdata/advancedsettings.xml`:

[sourcecode lang="xml"]
    <advancedsettings>
      <useddsfanart>true</useddsfanart>
      <cputempcommand>cputemp</cputempcommand>
      <samba>
        <clienttimeout>30</clienttimeout>
      </samba>
      <network>
        <disableipv6>true</disableipv6>
      </network>
    <loglevel hide="false">1</loglevel>
      <gui>
        <algorithmdirtyregions>1</algorithmdirtyregions>
        <visualizedirtyregions>false</visualizedirtyregions>
        <nofliptimeout>0</nofliptimeout>
      </gui>
      <measurerefreshrate>true</measurerefreshrate>
      <videoextensions>
          <add>.dat|.DAT</add>
       </videoextensions>
       <tvshowmatching append="yes">
        <!-- matches title 01/04 episode title and similar.-->
           <regexp>[s]?([0-9]+)[/._ ][e]?([0-9]+)</regexp>
       </tvshowmatching>
      <gputempcommand>/usr/bin/aticonfig --od-gettemperature | grep Temperature | cut -f 2 -d "-" | cut -f 1 -d "." | sed -e "s, ,," | sed 's/$/ C/'</gputempcommand>
    </advancedsettings>
[/sourcecode]

Did those and while they dropped the CPU utilization to about 25% which was quite good. However, during videos, the CPU was still high - and that's because even though XBMCBuntu official uses hardware acceleration through VAAPI, it still is spotty.

#### Getting XvBA
I went over to the XBMC-XvBA installation thread and followed the directions in the first post to add the XBMC-XvBA ppas. The download took some time and XvBA build got installed. Started XBMC and things were much, much better. 

[sourcecode lang="shell"]
    sudo apt-add-repository ppa:wsnipex/xbmc-xvba
    sudo apt-get update
    sudo apt-get install xbmc xbmc-bin
[/sourcecode]

There are other tweaks that are listed [on the XBMC-XvBA installation thread](http://forum.xbmc.org/showthread.php?tid=116996&pid=960883#pid960883) which I also went ahead and applied.

### Other tweaks 
#### Optimizing Linux for a flash/pen drive installation

Installing on a pen drive /usb flash drive has its pain points. My boot time was around painfully slow (~3.5 minutes). Opening Chromium took forever and even page loads were slow (it would be stuck with the status bar on 'checking cache'...). Also, the incessant writing to  disk is probably killing off my pen drive much much faster. I ended up doing the following:

1. Use the `noatime` and `nodiratime` flags for the USB drive 
[sourcecode lang="shell"]
    # /etc/fstab
    UUID=39f52ccf-363b-4b6e-abdd-927809618d83 /               ext4    noatime,nodiratime,errors=remount-ro 0       1
[/sourcecode]
1. Use tmpfs - In memory, reduces writes to disk and is faster. With 4G of RAM, this is a no-brainer.
[sourcecode lang="shell"]
    # /etc/fstab
    tmpfs /tmp tmpfs defaults,noatime,nodiratime,mode=1777 0 0
[/sourcecode]
1. Browsers - use [ profile-sync-daemon for Ubuntu](https://launchpad.net/~psycho-gabber-game/+archive/psd) from Arch Linux - will automatically move your browser profile directory from your home folder to /tmpfs 
1. Move `.xbmc` to NAS/External drive along with your media. Makes a lot more sense to keep your .xbmc folder with your media on a external hdd.
1. Change to `noop` or `deadline` scheduler:
[sourcecode lang="shell"]
    # Assuming sda is your USB drive
    sudo echo noop > /sys/block/sda/queue/scheduler
[/sourcecode]
1. Change system swappiness. We don't want the OS to use swap drive at all.
[sourcecode lang="shell"]
    # /etc/sysctl.conf
    vm.swappiness=1
[/sourcecode]

#### Getting suspend/hibernate to work

I had greatest trouble here - but was able to get `pm-utils` working eventually. `pm-utils` is a framework of shell scripts around suspend/hibernation/wakeup that provides hooks to execute scripts before standby/hibernation and when the computer resumes from sleep/shutdown. 
First test if basic suspend/hibernate works

[sourcecode lang="shell"]
    # check suspend methods supported
        cat /sys/power/state
    # S3
        sudo sh -c "echo mem > /sys/power/state"
[/sourcecode]

If your system goes into standby, then  things are good. But its just a good start. In my case, system would go into standby only the first time after boot. After that, it would go into standby but then resume immediately. Its been asked enough times on Google and I've probably tried all the fixes. The first one is to update a kernel param `acpi_enforce_resources=lax`
[sourcecode lang="shell"]
    # /etc/default/grub
    GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi_enforce_resources=lax"
[/sourcecode]

After that, make sure to run 
[sourcecode lang="shell"]
    sudo update-grub
[/sourcecode]
In my case, the magic incantation above failed (your mileage might vary). Nothing bad happened so I kept it on. Anyway, so I rebooted, then suspended and resumed the first time (which works) and took a dump of `dmesg > dmesg.1.log`. After that again tried to suspend and  when it came back immediately, I could get a dmesg output and scan the entries after the first run. Turned out that the log had entries related to xhci_hcd - so decided to unload it first and then  try to suspend:
[sourcecode lang="bash"]
    sudo modprobe -r xhci_hcd
    sudo sh -c "echo mem > /sys/power/state"
[/sourcecode]
After this, the system was able to standby each and every time. Now it was time to get pm-utils working. Out of the box, pm-utils came with a config that had a bunch of things that I didn't understand (and I doubt they applied to this machine). If standby was working directly, then it should have worked through pm-utils. However, it needed some amount of pushing around before that comes around to a functional state.

##### Getting `pm-utils` to play nice
So now that I had confirmed suspend working, it was time to see why `pm-utils` was being so bad. First off, time to clean up the default configuration. So copied `/usr/lib/pm-utils/config` to `/etc/pm/config.d/config` and then start editing it
[sourcecode lang="bash"]
    SLEEP_MODULE="kernel"
    # These variables will be handled specially when we load files in
    # /etc/pm/config.d.
    # Multiple declarations of these environment variables will result in
    # their contents being concatenated instead of being overwritten.
    # If you need to unload any modules to suspend/resume, add them here.
    SUSPEND_MODULES="xhci_hcd"
    # If you want to keep hooks from running, add their names  here.
    HOOK_BLACKLIST="99_fglrx 99lirc-resume novatel_3g_suspend"
    [/sourcecode]
     
##### Waking up with the keyboard
if you'd like wake up with a usb device (usb keybd), then you need to find out the usb port where your device is connected. The easiest way might be to check  dmesg output which would usually print this out. In my case, my wireless keyboard/trackball are connected on USB3

[sourcecode lang="bash"]
    echo USB3 >  /sys/proc/acpi
    echo  enabled > /sys/proc/devices/usb3/power/wakeup
[/sourcecode]

After that, the HTPC could be woken up with a keypress. Now I haven't been able to find a way to do the same thing with only the keyboard (so that the system doesn't wake up anytime anyone picks up the keyboard - so for now, have turned this off). The above change won't persist over a reboot - so to make it persistent, put the two lines above to `/etc/rc.local` before the `exit 0`

### Fixing up `fglrx` annoyances (ATI binary driver)

Not much point of a HTPC if the video isn't top quality. And there are a lot of variables involved there - your computer hardware, software, drivers, type of connection (HDMI/DVI) and the telly itself. Also, video driver support on Linux for ATI leaves quite a bit to be desired. One of the reasons of going with XBMCBuntu was knowing that there'll be large community support available on ubuntuforums.

Right off the bat, things started at the  _mildly irritating_ level. Catalyst control center in root mode won't start even though there's a big fat menu item there. Quick google and it says that the easiest way out is to  use `gksu amdcccle` in the run dialog (ALT-F2).

#### Full HD resolution
My telly (Panasonic 32") came with a native resolution of 1360x768 - was pleasantly surprised when I found that with a HDMI connection it would let me go up to 1920x1080. Interestingly, even the telly on screen menu reports full HD resolution - so I'm not really sure if they did put a full HD panel and report it as 720p normally. In any case, I'm not complaining and Full HD videos do look better :)

## So where does all this get us
After all this, it makes a sea change in the overall experience:

1. XBMC idles at 15 - 20% cpu utilization. During video playback, stil stays at a comfy 40% - 50% while playing 720p/1080p videos
1. Browsers (Chrome and FF) open near instantly; browsing experience is better than my desktop and page loads, tab switches etc feel much nimbler than on my desktop (AMD 6 core, 12G monster running Win 7x64)
1. Total cost - USD 180

### More to come

1. Hibernation support 
1. Torrenting
1. Scheduled wake up from shutdown/hibernate/suspend state


