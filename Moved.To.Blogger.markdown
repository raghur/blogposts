### Moving from wordpress

So I moved my blog from its old home at http://niftybits.wordpress.com to http://blog.rraghur.in 
I've also moved out of wordpress.com to blogger. For quite some time, I've not been happy with Wordpress's abilities for a tech blog. It's a commercial endeavour and so if you need additional features/tweakability you've got to fork out the good stuff.
Anyway, I was intending to get my own domain and wordpress hosting that would give me full control over the blog engine and the ability to install any addons that I wanted.
There was this niggling feeling in the back of my head that probably I was creating a monster - basically, set it up and then take on maintenance of it as well :(. That's part of the reason I decided to dust off my old blogger account and see where it stood. Now, the last time I'd touched Blogger about 10 yrs ago, it was just after Google bought over Blogger and was really more mommy blog engine. Things have changed and I've been living in a hole... Blogger's now much more polished. A few things where it leaves Wordpress.com in the dust:

1. Custom markup and css.
2. Custom domains
3. Google Analytics
3. Ability to have ads and hence make some money - not that I intend to.

Where it falls behind:

1. Analytics - really... the site analytics seems a little iffy.
2. Referrer spam - All I saw was entries of www dot vampire dot stat domain on the analytics dashboard. Turns out that its referrer spam and isn't blocked/can't be blocked. What I don't understand is how this was never a problme with wordpress.com.
3. Themes - far lesser than WP - but this is not an issue since you can tweak anything to your heart's content.

Now all of the WP shortcomings can be addressed if you go for paid upgrades OR just host your own WP. Unfortunately, neither is a good option for me. So while I didn't like moving out, it had to be done.

### Registering a domain
I went ahead and registered the domain with BigRock.in - here's a referral link that will [get you 25% off your domain](http://www.bigrock.in/?coupon=rraghur.in).

### Pointing Blogger to your custom domain
This was very simple - just follow the instructions on Blogger. You will need to set up 2 CNAME records for Blogger in your DNS management console other than your custom domain itself. For ex:

### Migrating content from WP.com
Also, had to get the blog content transferred. Went into wordpress dashboard Tools > Export > All Content and exported the blog. This lets you download a xml file with all your blog content. 
Now, we have to convert it so that it can be imported by Blogger. Move over to [Wordpress2Blogger](http://wordpress2blogger.appspot.com/) and upload the file. If all goes well, then you get a converted file. Wasn't so simple for me though and it gave a `Invalid XML at line xxxx` error. Opening the wordpress XML, I didn't see any issues and a little google indicated that WP.com is notorious for generating invalid/malformed XML. Hmm - not a biggie. Pulled out XMLLint  and ran it over the wordpress file 

~~~{class="bash"}
xmllint -v /path/to/wordpress/export/file
~~~

And `xmllint` found the problem - it was basically an `&nbsp;` entity that had not been declared. Just removed it from the file and uploaded again to Wordpress2Blogger and the conversion went without a hitch.


### Other Tweaks
Formatting of posts - while the content came over fine, it came in with `&lt;br /&gt;` tags. Also all my old source code listings that had the WP.com syntax tags `[sourcecode] [/sourcecode]` were broken and had to be fixed.

For code syntax highlighting, I went with [hightlight.js](http://softwaremaniacs.org/soft/highlight/en/) for now. May change later. Also copied the key hightlighting css from [here](http://meta.stackoverflow.com/questions/26207/how-can-i-format-as-keyboard-keys). You can edit the template and stick in the markup in the head section.

Next, spent some time on the theme tweaks on Blogger till I got tired. I'm ok with it for now - but will probably come back to it later.

### Closing
Well, the move was completed. I'm a little dissatisfied with a few things:

2. What do I do with my wordpress blog. I cannot redirect it and I can delete it but a little hesitant about burning bridges.
1. Reaching via google search seems to be a problem - searching for `xbmc xvba nettop rraghur` doesn't even show up the Blogger link. Only the wordpress links are there. I think this is because of page reputation - so not much I can do.
3.  Blogging with VIM: Vimrepress was a good solution for WP.com. For blogger I have only found [Blogger.vim](https://github.com/ujihisa/blogger.vim) which I'm still to give a try.

The benefits are worth much more and having the peace of mind of not having to worry about maintenance and the freedom that I can move to a self hosted blog later on if needed is indeed good.



