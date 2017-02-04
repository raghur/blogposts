<!--
PostId: 2594766479112048632
Title    : Posix compatible PTY in Windows
Labels   : Windows, linux, shell
Format	 : markdown
-->
### Get a real Posix compatible PTY in Windows

If you do any work on hte console in WIndows and are not on Windows 10, then you should probably use Conemu.

If you're a linux shell fan and are using Msys2 or Cygwin on windows, then an additional tweak is to enable
the experimental Posix compatible PTY support in ConEmu.

I've been a long term ConEmu user and did not know about this till yesterday. Enable it and you get full
xterm-256color TERM.

So `cat $ConEmuBaseDir/Addons/AnsiColors256.ans` now produces
![img](https://i.imgur.com/FlnOrYQ.png)

Here's how the tasks are set up in ConEmu - this one's for MSYS2

```
set MSYSTEM=MINGW64 & set MSYS2_PATH_TYPE=inherit & set HOME=c:\users\raghuramanr & d:\msys64\usr\bin\conemu-msys2-64.exe /usr/bin/zsh -l -i

```

[See here for directions](http://conemu.github.io/en/CygwinMsysConnector.html)

