<!--
PostId: 1389082483683946885
Title    : Zsh constantly crashes on Msys64
Labels   : shell,linux, zsh
Format	 : markdown
-->

So, I wanted to try out Zsh as a bash replacement after using it on and off. Now I don't write a lot of scripts - but mostly
for running apps etc. The only real improvement that I needed was cross shell history... I can never remember where I typed
a command and hitting <kbd>Ctrl-R</kbd> and finding the command is super nice.

On my msys64 install, did a `pacman -Su zsh` and zsh was installed. Now, since I'm on pre windows 10, everything's a little more painful if you want to run bash/linux tools

Anyway, since I use Conemu as my shell, had to change my Conemu Task to something like

```
set MSYSTEM=MSYS & \
set MSYS2_PATH_TYPE=inherit & \
set HOME=c:\users\raghuramanr & \
d:\msys64\usr\bin\conemu-msys2-64.exe usr\bin\zsh -i -l
```

Everything was great. Also installed a zsh customization framework - [zprezto](https://github.com/sorin-ionescu/prezto)
and had a decent prompt, reverse history search going on. Everything was good for a couple of days - but then I couldn't
leave it at that, could I?

And then, I did a `pacman -Su` - this updated msys runtime. Now zsh segfaults with an access violation at startup. Starting
a blank `.zshrc` works - so definitely some init scripts were causing it.

Tried a bunch of things - getting rid of `zprezto` (github stars nonetheless, it seems unmaintained - and was hoping
that a replacement would fix the crash);  replaced it with [Zim - *Zsh Improved*](https://github.com/Eriner/zim).
That didn't help either. Got rid of it as well and started customizing zsh by hand. What I found that running `compinit`
crashes zsh everytime - so finally gave up and am back to bash

