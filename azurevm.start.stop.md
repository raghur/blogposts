<!--
PostId: 874439820067701439
Title    : Azure VM - start/stop using automation
Labels   : azure, automation
Format	 : markdown
-->
So this is just sharing my long time azure automation script to start/stop a vm. I have this on a schedule
so it starts on weekdays in the morning and is shutdown in the evening. There's also a
couple of webhooks so I can start/shutdown using my phone.

The latest addition is integrating push notifications to my phone - I have a [Pushover](http://pushover.net)
which offers a nice api to send push notifications to your devices.

<script src="https://gist.github.com/raghur/1fef6c8c128193f7893abe5aeb5f45ce.js"></script>

Note that to set this up, you will have to set up a user in Azure AD and add those credentials to automation assets.

I absolutely love the push notifications :)
