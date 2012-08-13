"=========== Meta ============
"StrID : 488
"Title : Coffeescript rocks!
"Slug  : Coffeescript-rocks
"Cats  : Tips, Very Cool
"Tags  : Javascript, Coffeescript, Tips, development
"=============================
"EditType   : post
"EditFormat : Markdown
"TextAttach : /home/wpcom/public_html/wp-content/blogs.dir/8fe/814317/files/2012/08/vimpress_5024ccbb_mkd.odt
"========== Content ==========

I've been absent a few weeks from the blog. Life got taken over by work - been deep in the Javascript jungles and Coffeescript has been a lifesaver.

Based on my earlier [peek at Coffeescript](http://niftybits.wordpress.com/2012/03/13/coffeescript-looks-promising/), we went ahead full on with Coffeescript and I have to say it has been a pleasant ride for the team with over 4.7KLoc of Javascript (with Coffeescript source weighing in around 3.7KLoc including comments etc) that now I can confidently recommend it for any sort of Javascript heavy development.

I'm going to list down benefits we saw with Coffeescript and hopefully someone else trying to evaluate it might find this useful:

1. Developers who haven't dove deep into Javascript's prototype based model find it easier to get up to speed sooner. Yes - once in a while they do get tripped up and then have to look again into what's going under the covers - but this is normal. The key point is that its much much more productive and enjoyable to use Coffeescript. 
1. The conciseness of the Coffeescript definitely goes a long way in improving readability. One of the algorithms implemented was applying a bunch of time overlap rules. We also used [Underscore.js](http://underscorejs.org) - and between Coffeescript and Underscore.js, the whole routine was within 20 lines, mostly bug free and very easy for new folks to pick upand maintain over time. Correspondingly, the generated JS was much more complicated (though Underscore helped hide some of loop iteration noise) - and it wouldn't have been too different had we written the JS directly.
1. Integrating with external frameworks - jquery, jquery ui etc was again painless and simple.
1. Another benefit was that the easy class structure syntactic sugar helped quickly prototype new ideas and then refine them to production quality. With developers who're still shaky on JS, I doubt the same approach would have worked since they'd have spent cycles trying to get their heads wrapped around JS's prototype based model.
1. Coffeescript also allows you to split the code to multiple source files and merge all of them before compiling to JS - this allowed us to keep each source file separate and reduce merges required during commits.
1. Finally, performance is a non issue - you do have to be a little careful otherwise you might find yourself allocating function objects and returning them back when you don't mean to but this is easily caught in reviews.

One latent doubt I had going into this was the number of times we'd have to jump in to the JS level to debug issues. With a larger Coffeescript codebase spread across multiple files, this is a real concern since the error line numbers wouldn't match with source and if we have to jump through hoops to fix issues. Luckily, this wasn't a problem at all - over time, in cases of either an error in JS or just inspecting code in the browser, its easy to map to the Coffeescript class/function - so you just fix it there and regenerate the JS. Secondly, the generated JS is quite readable - so even when investigating issues, it's quite trivial to drop breakpoints in Chrome and know what's going on.

The one minor irritation was if there was a Coffeescript compile issue, then when joining the file, the line number reporting.fails and then you have to compile each file independently to figure out the error. Easily automated with a script - so that's just being nitpicky.

Anyway, if you got here looking for advice on using Coffeescript, then you've reached the right place and maybe this post's helped you make up  your mind!


