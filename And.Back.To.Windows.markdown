"=========== Meta ============
" StrID : 654
" Title : And we're back to windows
" Slug  : And-we're-back-to-windows
" Cats  : Uncategorized
" Tags  : linux, virtualbox, wine, DRM
"=============================
" EditType   : post
" EditFormat : Markdown
" TextAttach : 
"========== Content ==========

#### And back to Win 7
Well not really - but I have your attention now... So in my [last post](http://niftybits.wordpress.com/2013/08/09/upgraded-to-linux/), I talked about moving my home computer from Win 7 to Linux Mint KDE. That went ok for the most part other than some minor issues.

Fast-forward a day and I hit my first user issue :)... wife's workplace has some video content that is distributed as DRM protected swf files that wil play only through a player called [HaiHaiSoft player](http://www.haihaisoft.com/hup.aspx)!

#### Options
1. Boot into windows - painful and slow and kills everyone's else session.
2. Wine - Thought it'd be worth a try - installed Wine and dependencies through Synaptic. As expected, it would'nt run haiHaiSoft player - crashed aat launch.
3. Virtualization: so the final option was a VM through virtualbox. Installed Virtualbox and its dependencies (dkms, guest additions etc) and brought out my Win 7 install disk from cold storage.

#### Virtualbox and Windows VM installation
Went through installation and got Windows up and running. Once I got the OS installed, also installed guest additions and it runs surprisingly well. I'd only used Virtualbox for a linux guest from a Windows host before so it was a nice change to see how it worked the other way around.

Anyway, once the VM was installed, downloaded and installed the player and put a shortcut to virtualbox on the desktop. Problem solved!
