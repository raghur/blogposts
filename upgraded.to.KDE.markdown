<!--
"=========== Meta ============
" StrID : 642
" Title : Upgraded to Linux
" Slug  : Upgraded-to-linux
" Cats  : Uncategorized
" Tags  : linux,mint,KDE
"=============================
" EditType   : post
" EditFormat : Markdown
" TextAttach : wpid644-vimrepress_5205e822_mkd.odt
"========== Content ==========
-->

So after suffering tons of crashes (likely due to AMD drivers) and general system lagginess, I finally decided to ditch windows and move to linux full time. 
This is on my home desktop which is more a family computer than something that only I would use.
I was a little apprehensive with driver support as usual and tricky stuff like suspend to ram (s3) which always seems highly driver dependent and problematic  on Linux (it is still a pain on my XBMCBuntu box). Anyway, nothing like trying it out.

After looking around a bit, downloaded Linux Mint 15 (default and KDE). Booted with the Live CD and liked the experience - though GNOME seems a bit jaded and old. I liked KDE much better - esp since it seems more power user friendly.

So after testing hardware stuff (Suspend, video drivers and so on) - all of which worked flawlessly, I must say, I decided to go ahead and install it on one of my HDDs. Unfortunately, installation was rocky a bit - I don't know if it was just me - the mint installer would progress upto preparing disks and hang there for 10+ minutes without any feedback I'm assuming it is reading partition tables and so forth - but no idea why it took so long. A thought it'd hung a couple of times - so terminated it and it was only accidentally that I found that it was still working - when I left it on its own for sometime and got back. It presented my the list of options (guided partition on entire disk, co locate with another OS etc) - but things actually went worse after this.

What seems to have happened is that my pending clicks on the UI all were processed and it proceeded to install on my media drive before I had a chance ... wiped out my media drive. Thankfully, before installation I had a backup of the important stuff on that drive and so it wasn't a biggie...
At this point, I was having serious doubts of continuing with Mint and was ready to chuck it out of the window and go back to Kubuntu or just back to Windows. However, I hung on - given that I'd wiped a drive, might as well install it properly and then wipe it if it wasn't good.

Anyways, long story short, I restarted the install, picked my 1TB drive and partitioned it as 20GB /, 10Gb /var, 1Gb /boot and remaining as unpartitioned.
Mint went through the installation and seemed to take quite sometime - there were a couple of points where the progress bar was stuck at some percentage for
multiple minutes and I wasn't sure if things were proceeding or hung. In any case, after the partitioning window, I was more inclined to wait. Good that I did since the installation did eventually complete. 

Feedback to Mint devs - please make the installer be more generous with feedback - esp if the installer goes into something that could take long.

### First boot ###
Post installation, rebooted and grub shows my windows boot partition as well as expected. I still haven't tried booting into windows so I that's one thing to check.
Booted into Mint and things looked good. Set up accounts for my dad and my wife. One thing I had to do was edit `/etc/pam.d/common-password` to remove password complexity (`obscure`) and set minlen=1 

         password	[success=1 default=ignore]	pam_unix.so minlen=1 sha512
         
Next was to set up local disks (2 ntfs and 1 fat32 partition) so that they are mounted at boot and everyone can read and write to them. I decided to go the easy route and just put entries in `/etc/fstab`

	UUID=7D64-XXX  /mnt/D_DRIVE    vfat      defaults,uid=1000,gid=100,umask=0007             		0       2
	UUID="1CA4559CXXXXX" /mnt/E_DRIVE ntfs rw,auto,exec,nls=utf8,uid=1000,gid=100,umask=0007             	0       2
	UUID="82F006D7XXXX" /mnt/C_DRIVE ntfs rw,auto,exec,nls=utf8,uid=1000,gid=100,umask=0007             	0       2

That fixed the mount issue but still need to have them surface properly on the file manager (dolphin) - this was actually quite easy - I just added them as places, removed the device entries from the right click menu. This worked for me - I'd have liked to make this the default but didn't find a way. Finally decided to just copy the `~/.local/share/user-places.xbel` file to each local user and set owner.

###Android###
Other than that, I also need to be able to connect my nexus 4 and 7 as MTP devices. I had read that this doesn't work out of the box - but looks like that's been addressed in ubuntu 13.04  (and hence in Mint)
I also need adb and fastboot - so just installed them through synaptic. BTW, that was awesome since it means that I didn't have to download the complete android SDK just for two tools.

### General impressions ###
Well, I'm still wondering why I didn't migrate full time to linux all these years. Things have been very smooth - but I need to call out key improvements that I've seen till now

1. Boot - fast - less than a minute. Compare that to up to 3 mins till the desktop is loaded on Win 7
2. Switching users - huge huge speed up. On Windows, it would take so long that we would most of the times just continue using other's login.
3. Suspend/Resume - works reliably. Back in Windows, for some reason, if I had multiple users logged in, suspend would work but resume was hit and miss.
4. GPU seems to work much better. Note here though that I'm not playing any games etc. I have a Radeon 5670 - but somehow on windows even Google Maps (new one) would be slow and sluggish while panning and zooming. Given that on Linux, I'm using the open source drivers instead of fglrx, I was expecting the same if not worse. Pleasantly surprised that maps just works beautifully. Panning,zooming in and out is smooth and fluid. Even the photospheres that I had posted to maps seem to load a lot more quickly.


Well, that's it for now. I know that a lot of it might be 'new system build' syndrome whereas on windows gunk had built up over multiple years. However, note that my windows install was fully patched and up to date. Being a power user, I was even going beyond the default levels of tweaking (page file on separate disk from system etc) - but just got tired of the issues. The biggest trigger was the GPU crashes of course and here to updating to latest drivers didn't seem to help much. I fully realize that its almost impossible to generalize things. My work laptop has Win 7x64 Enterprise and I couldn't be happier - it remains snappy and fast in spite of a ton of things being installed (actually, maybe not - the Linux boot is still faster) - but it is stable.
And of course, there might be a placebo effect at some places - but in the end what matters is that things work.



