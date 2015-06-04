<!--
PostId: 1501155548936656796
Title    : Neovim on Windows 7
Labels   : vim
Format   : markdown
-->
### Neovim on Windows

For folks who dig vim as an editor, you have most likely heard of Neovim - a fork of the vim with the goal to modernize the codebase and add unit tests.

In fact, on Linux, the editor's been functional for quite some time and I've replaced vim with neovim - and I've hardly ever come across a serious issue. My original [dotvim](https://github.com/raghur/vimfiles) works flawlessly with all 54 plugins loaded and running!

Unfortunately, Windows is a different story - as usual, things are more difficult. However, contributor [equalsraf](https://github.com/equalsraf) has been stellar work on the Windows port. There's a CI build as well at Appveyor - however, on Windows since the native tui doesn't work, you need a UI plugin.

A few days ago, I stopped again to check where things were and with a little wrangling, got Neovim on Windows! So I posted on Reddit, then found a few issues, and /u/equalsraf got them fixed and we went back and forth! So folks, if you'd like to see Neovim on Windows, you can try it out and report issues and bugs which will definitely help the effort.

Here's how it works - Electron (on which Github's atom editor is built ) is used as the UI and it drives the neovim executable on windows

```bash

# install electron prebuilt  and grunt systemwide using node js
npm install -g electron-prebuilt grunt-cli

# clone this repo
git clone https://github.com/coolwanglu/neovim-e

# inside the repo, install dependencies
cd neovim-e && apm install && grunt

# grab the neovim from appveyor https://ci.appveyor.com/project/equalsraf/neovim
# unzip it to someplace on your path.

# Launch - from inside the folder where you cloned neovim-e
Electron .

# init files
# copy ~/.vim to ~/.neovim
# copy ~/.vimrc to ~/.neovimrc

```
You can also create a shortcut to neovim - just create a batch file in the folder to launch `electron .` and add a shortcut to the batch file.

One important tweak - getting functional copy/paste - Grab paste.zip from [here](http://www.c3scripts.com/tutorials/msdos/paste.zip). Unzip and rename it to `pbpaste.exe`. Also get `clip.exe` - should be on your `system32` folder and put it somewhere on your path as `pbclip.exe`

Given that it's pre-alpha and not even an official build, just the fact that it launches and is usable to this extent is promising. Unite, Ctrlp and the most majority of my plugins work. There's an outstanding issue with quoting and escaping shell executable arguments - and that means that python plugins don't work yet (for ex: barfs if I enable Ultisnips - so I've disabled it for now)

Other than that, it's pretty good - and any bug you run into will only help make it better for everyone!




