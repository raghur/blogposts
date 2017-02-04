<!--
PostId: 769597026826152808
Title    : Minikube on Windows - teething troubles
Labels   : kubernetes, windows, tips, rant
Format	 : markdown
-->

### The `.minikube` folder location

So I was recently getting [Minikube](https://github.com/kubernetes/minikube) on
my work laptop. As it happens, this did not go painlessly. My C drive has
a measly 100G (free 8G) so almost all tools I install, if their dot file folder
is going to hold anything significant like virtualbox images, I usually symlink
it off to my D drive which is a lot roomier.

DON'T DO THAT WITH MINIKUBE YET.... it does not work :(. I tried symlinking the
`.minikube` folder when that didn't work, then the `.minikube/machines` folder
- neither worked. If you're really desperate, you could use Virtualbox media
manager to move the disk to some other drive. Of course, this isn't really
a solution since you need to have enough space in C in the first place.

There's a ticket to have
a [`MINIKUBE_HOME`](https://github.com/kubernetes/minikube/issues/1041) which
will let you move the config folder around - so go subscribe to it if you need
this. The second PITA is that the `minikube.exe` as well as the `kubectl` all
have to be on C drive as well - but symlinks work for that.

### SSH client

I also ran into trouble with ssh - minikube vm provisioning was using my
external SSH client that it found on `PATH` and was calling that without
quoting properly which resulted in the provisioning failing. Turning on verbose
logging with `--v 9` showed what was happening and the easiest way out was to
make sure there's no `ssh.exe` on path  - this forces minikube to use it's
internal ssh client and provisioning proceeded.

Once you have Minikube up though, it's quite nice to work with.

