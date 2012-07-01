"=========== Meta ============
"StrID : 463
"Title : Avast! trial expiration kills Internet connection - how bad is that!
"Slug  : Avast!-trial-expiration-kills-Internet-connection---how-bad-is-that!
"Cats  : Troubleshooting
"Tags  : avast,rants
"=============================
"EditType   : post
"EditFormat : Markdown
"TextAttach : /home/wpcom/public_html/wp-content/blogs.dir/8fe/814317/files/2012/06/wpid-homewpcompublic_htmlwp-contentblogs-dir8fe814317files201206wpid-homewpcompublic_htmlwp-contentblogs-dir8fe814317files201206vimpress_4feec608_mkd.odt
"========== Content ==========
 
So I use 'free for personal use' [ Avast Antivirus ](http://www.avast.com/free-antivirus-download) at home for the past couple of years. It's been mostly good though I've had some reservations about it - namely, nag pop-ups and so on. Some months ago (or maybe was it a year ago?) there was a program update and it wanted me to install 'Avira Internet Security'. Now I had no need for this (I use COMODO firewall which has been quite good) however, there was no way around it. Avira's update process said I could revert back to the free Antivirus version anytime without a re-install or a re-anything!

Not much of an option and you can't blame them for trying to push their products and convert free users to paying ones - so I went ahead with the upgrade. About a fortnight ago, I started getting warnings about  'your trial licenses is about to expire' and so on. The good thing about the internet security product was that it was discreet - in fact, safe to say that I even forgot that I installed it.

Remembering the notices about the trial expiring and reverting back to the free version, I chose to ignore all the warnings till yesterday afternoon when the wife called me up at work about 'internet not working from the home machine'. Now my wifi dongle on the home pc does once in a while show up with a 'Limited connection' that's quickly fixed with either disabling and enabling the dongle or unplugging it and putting it back in the USB port. I offered that up as a solution and a few hours later am told that it hasn't fixed the issue.

This morning I finally sat down to see what was up. Turns out that the WIFI just wouldnt connect. So up comes device manager and under Network Devices I see a whole lot of 'avast! NDIS filter' virtual devices showing up. Opened Avast! gui and there are no panels for turning the thing off... Its reverted back to the free version - but has killed my net connection in the process! Not a happy camper at this point - still wasn't worried since I figured there've got to be tons of users with the same problem and it probably has a simple fix. Google did not reveal any simple fixes - Avast's community forum had [help! Avast Internet Security trial expired, no internet connection](http://forum.avast.com/index.php?topic=91512.0) and [ Avast Internet Security Trial seems to have affected my internet connection](http://forum.avast.com/index.php?topic=99736.0). The suggestions offered - buy a license, uninstall and re-install Avast etc were just not Ok. I definitely wasn't the only one affected but looks like a small but sizable user population was affected. If that was so, then Avast! should have done something about it - however, looks like they don't believe much in that. Agreed it's a free product and will not merit levels of support that you'd get for something that you pay for. But:

1. I did not ask for the installation of the 'Premium' product trial.
1. There was no option to opt out of the 'trial'
1. They actively messaged that there's 'nothing to lose' from using the trial.

Given all that, they should have stepped up and either taken care of the issue with an update OR put up steps on how to solve it. It doesn't take that much. Here's how I got back my net connection:

1. Device manager - Remove all 'Avast!' virtual devices with a right click 'Uninstall'
1. Restart
1. No WIFI still... so open 'Network and sharing center-> Change Adapter settings-> Wifi Connection -> properties'. In the 'This connection uses the following items:' list there was one more avast! filter device. Selected and uninstalled  that too and restarted again.
1. Back in business...WIFI is back up and running!

It's time to say goodbye to Avast!. Any recommendations for good, free antivirus solutions?


