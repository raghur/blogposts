<!--
PostId: 7823551512555885194
Title    : Neovim on Windows
Labels   : vim, neovim, tips, howto
Format	 : markdown
Published: true
-->

If you're a fan of Vim, then no doubt you've heard of Neovim - in fact, neovim
has been a key driver in much of the changes that have made it into the latest
version of vim (async, job control etc).

Neovim works extremely well as a daily driver on Linux - but for Windows
support has been lagging. I've been using a fork from contributor [equalsraf](http://github.com/equalsraf/neovim)
who's been behind a lot of the windows support.

Last week, the main neovim project gained windows CI builds on appveyor. The
instructions for installation were also updated on the wiki - so obviously, I
was quick to try it. The windows appveyor builds also bundle the frontend -
`neovim-qt` - however, there seems to be an issue that not all of `neovim-qt`
runtime files are being included correctly in the build. Due to this, one
cannot set `GuiFont` and so on and this is a bummer. Anyway, it's easy to fix -
just get the [neovim-qt binaries] from equalsraf's appveyor and unzip them
over.
![upating neovim](https://i.imgur.com/vOLKCB2.gif)

Since neovim is now on master and changes are coming in fast and thick, I wrote
a little powershell to update my neovim installation. Here it goes for your
enjoyment (you need powershell v5 for this)

**UPDATE - 2017-03-01** - The bug in question
<https://github.com/neovim/neovim/issues/6145> is now fixed in Neovim windows
builds. So in the script below you can leave out pulling  down neovim-qt
separately

``` {.powershell}
param (
    [string] $loc="d:\utils\"
      )
wget "https://ci.appveyor.com/api/projects/neovim/neovim/artifacts/build/Neovim.zip?branch=master&job=Configuration%3A%20MINGW_64" `
    -OutFile "$env:TEMP\neovim.zip"
wget "https://ci.appveyor.com/api/projects/equalsraf/neovim-qt/artifacts/neovim-qt.zip?branch=master&job=Environment%3A%20SCRIPT%3Dcontrib%5Cappveyor-msvc.bat" `
    -OutFile "$env:TEMP\neovim-qt.zip"

expand-archive -Path "$env:TEMP\neovim.zip" -DestinationPath $loc -Force
expand-archive -Path "$env:TEMP\neovim-qt.zip" -DestinationPath "$loc\neovim" -Force
```



