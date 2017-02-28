<!--
PostId: 7568278269117030161
Title    : Password utopia - finally!
Labels   : tools, utitlities, tips
Format	 : markdown
-->
### Passwords - what a pain.

Bajillions of websites, different usernames, work projects and their environments and their passwords...how the hell are you supposed to keep track of all of it. And then a dozen different machines that you work on... and need access to your passwords

Each of us have their own mechanisms - plain text file 'hidden' some place to slightly more advanced ones. For the longest of times, I used to have a GPG encrypted text file sync'ed over my github repository. It was simple and did the job though used to be a little painful to get started off on a new machine (needed to have my gpg keys etc and git set up and so on.). Also, it didn't help in the password generation department

In between, I've played with other tools like pass - but then cross platform is a must and that didn't work. Purely command line is good but a pain to deal with.

So here's what my password-utopia looks like:

1. passwords are automatically synced
2. accessible anywhere - linux, windows, android and CLI interface
3. Key secured if possible with GPG
4. search over saved passwords
5. Browser helpers to automatically fill in usernames/passwords
6. Generate strong passwords

Keepass2 does all of the above - after initially dabbling with it, I've now moved all my passwords on to it. you can use Dropbox, Google drive or any other cloud sync tool to sync your passwords across machines. `kpcli` provides cli access and you have `KeepassDroid` on android. There's a mono version for Linux but I found that slow and clunky. `KeepassX` nightly ppa has support for kdbx database files and while a little lacking in features, it works much better for me on Linux.

Securing the Keepass2 database - you can use a password and a key file combination. You could also use your windows account but then I think cross platform compatibility is lost. anyway, random key file + strong master password is quite ample for my needs.


