<!--
PostId: 8706379307041798323
Title    : Torrent download notifications with Pushover
Labels   : shell, linux
Format	 : markdown
-->

Last weekend I setup torrent complete push notifications to be delivered via pushover. I have my media center box running
ubuntu 16.04 with Kodi. It also serves as my torrent box with qbittorrent.

On Android Transdroid is great to manage torrents but it doesn't have auto refresh. Given that I rediscovered my
pushover account last week, it seemed just begging to be used.

Here's a shell script to send push notifications - just set it up in in qbittorrent (or whatever you use) to run
at torrent completion - like so `/bin/user/bin/push.sh "%N" completed`

<script src="https://gist.github.com/raghur/93da463d3a837699b2a20427f4cf1335.js"></script>


