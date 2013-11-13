<!--
PostId: 6580438869065530077
Title    : Vim: Making Ultisnips and NeoComplete play nice
Labels   : vim, troubleshooting
Format	 : markdown
-->
### Plugin conflict!
If you have both Ultisnips and NeoComplete then you cannot use the same key for expansion. I used to have <kbd>tab</kbd>
mapped out to both for completion with AutoComplPop and Ultisnips. I had <kbd>tab</kbd> set for `g:UltiSnipsJumpForwardTrigger`
but NeoComplete still doesn't like it.
So now, that's changed to <kbd>Control</kbd> + <kbd>Tab</kbd> and things are good again.

~~~{class="bash"}
let g:UltiSnipsExpandTrigger="<C-CR>"
let g:UltiSnipsJumpForwardTrigger="<C-tab>"
let g:UltiSnipsJumpBackwardTrigger="<s-tab>"
~~~

Only wish one NeoComplete OR Ultisnips makers see this and make it work without conflict




