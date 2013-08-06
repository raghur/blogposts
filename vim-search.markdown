"=========== Meta ============
"StrID : 
"Title : Vimgrep on steriods - even on Windows
"Slug  : Vimgrep-on-steriods-even-on-Windows
"Cats  : Uncategorized
"Tags  : vim,vimscript
"=============================
"EditType   : post
"EditFormat : Markdown
" TextAttach : vimrepress_51f15fd7_mkd.odt
"========== Content ==========

So I was looking at [this vim tip](http://vim.wikia.com/wiki/Find_in_files_within_Vim) for finding in files from within Vim - while it *looks* helpful, there are a number of possible improvements:

1. Why a static binding? being able to tweak the patterns or the files to search is quite common - so much more value if you could have the command printed in the command line, ready to be edited to your heart's content or just go ahead and execute the search with `[Enter]`.
1. The tip wont work for files without extensions (say .vimrc) - in this case, `expand("%:e")` returns empty string
1. `lvimgrep` is cross platform but slow - let's use Mingw grep too for `vimgrep`
1. And make that Mingw grep integration work on different machines

It was more of an evening of scratching an itch (a painful one if you're zero in vimscript :) ). Here's the [gist](https://gist.github.com/raghur/6081804) gist for it- hope someone finds it useful.

Feel free to tweak the mappings - I use the following:

1. leader-f: normal mode: vimgrep for current word, visual mode: search for current selection
1. leader-fd: Similar - but look in the directory of the file and below
1. leader-*: Similar to the above, but use internal grep

Save the file to your .vim folder and source it from .vimrc
[sourcecode language="viml"]
    so ~/.vim/grephacks.vim
[/sourcecode]

A few notes:

1. GNUWIN is an env variable pointing to some folder where you've extracted mingw findutils and grep and dependencies
1. The searches by default work down from whatever vim thinks is your present working directory. I highly recommend [vim-rooter](https://github.com/airblade/vim-rooter) if you're using anything like subversion, mercurial or git as vim-rooter automatically looks for a parent folder that contains .git, .hg or .svn (and more - please look it up)

Happy vimming!
