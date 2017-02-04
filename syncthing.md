<!--
PostId: 5448701899878581480
Title    : Syncthing - run your own sync server
Labels   : tips, utilities, cloud-storage
Format	 : markdown
-->

### What I'm after

I'm trying out [syncthing](http://syncthing.net) - I've become frustrated that none of the other options seem to tick
all the boxes of my needs. Here's what I need and it doesn't seem that I'm asking for a lot here:

1. Bi directional file syncing
5. Security & privacy - at rest and in motion
1. Windows client
1. Linux desktop client
2. Android client.

### The competitors
Here's what I've tried so far:

1. Dropbox - fails on [privacy](http://www.theinquirer.net/inquirer/news/2356086/snowden-says-dropbox-is-hostile-to-privacy)
2. Google Drive - no linux desktop client - so a non starter.
3. OneDrive - ditto AFAIK
3. Mega - Android client won't autosync changes.

And so I've been looking at setting up my own sync/cloud storage. I used to run [BTSync](https://www.resilio.com/individuals/)
but dropped it since it wasn't open source. Apparently, it's now renamed to 'Resilio'. I've also been interested in
[OwnCloud](http://owncloud.org) - but it seems an overkill - I don't need calendaring and such.

I've also heard good things about seafile as well - but that will have to wait for another day.

### Syncthing - installation
So today being a holiday, thought of giving syncthing a try. It's basically an open source BTSync like application. It
looked interesting and seemed to tick all my boxes so worth a try. However, installation wasn't as smooth - and getting
it to work

1. Linux Desktop- Deb packages are available; setup was a snap. Getting it configured and running as a systemd service took some going
through the [online manual](https://docs.syncthing.net/users/autostart.html#using-systemd)
2. Windows - Used [SyncTrayzor](https://github.com/canton7/SyncTrayzor). Getting it installed wasn't problematic.
3. Android - there's a syncthing app on the play store. It works but seems a little crashy (clicking on the folder icon always crashes it.)
4. Linux server - so I have a cloud vm and decided to have each of the machines above sync with it... so I can make changes anywhere
and I can get it on the other devices. Once again, after I added the apt source, getting syncthing installed was a breeze.

### Configuration

And now coming to configuration - this is where I feel Syncthing's UX will scare away most people (probably except those
who're just running windows desktops). Here's my nits:

1. Adding a remote machine - you need to provide a 50- 55 key machine Id. In my case, I needed to provide the key of my
headless server to the local linux desktop service. This in itself wasn't a problem - I just picked the key from the logs on
the server `journalctl -e -u syncthing.service` and pasted it. Thing is, that once you add a remote machine, you then
get prompted on the web interface that a machine wants to share. On my headless server, I could see my desktop trying to
connect (from logs) but it was waiting for authorization. I finally had to dig into syncthing's configuration syntax and
edit `~/.config/syncthing/config.xml` and add device id nodes for my desktop, windows machine and android phone and
restart each time after editing the config.
2. Syncthing's scheme is that not only does a remote machine have to be added, but you also have to specifically mark
what folders are shared with each machine. This means that you have to configure each machine with folders with teh same
`id` otherwise they won't sync. This caused some confusion - esp since the windows app decided to create a folder with a
random id where as the linux just had `default` as the id. I finally just went with `default` everywhere.
3. There's something called an `introducer`.. I read the description of what it's supposed to do and did not understand..
Better docs/graphic would help a lot here. For the moment though, I decided to stay away from it.

Anyway, after I sorted those out, it seems to be working... I tried editing files on Android and it got pushed to other nodes
and vice-versa. It feels a little sluggish in detecting changes - but I can live with that.

I also need to understand it's n-way sync process... maybe I can go peer to peer. Syncthing definitely has a
diamond in the rough feel to it and I'd rather support an open source project if it can job done (albeit with some initial
pain getting it set up)

### What can be done better

1. Getting a headless central node working painlessly would go a long way. I expect a lot of people are using syncthing
this way... something along the lines of :
    1. Get all your clients installed,
    2. install on headless syncbox;
    3. Provide a cli tool that will configure a device for the default folder.
        `syncthing-cli add folder=<folder-id> <device-id1>, <deviceid2>...`
        All this does is update your config & restart syncthing service. `folder` defaults to `default` if not specified.

It's open source - and given that I like what I see (for the most part), I'll probably try my hand at sending in a PR.

Happy syncing!

