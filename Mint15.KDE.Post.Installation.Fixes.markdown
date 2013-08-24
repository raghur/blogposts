## Additional fixes post installation

### SSH connection refused
So today while my dad was on the desktop and I was on the laptop, I tried ssh'ing into the desktop and no go. I was getting a connection refused and thought it had to do with either SSH not being installed or being blocked by the firewall.
Later on when I checked, OpenSSH server was installed and the service was also running

~~~{class="bash"}
sudo service status ssh
ssh start/running, process 2709
~~~

Hmm - this is weird. Next check was for `iptables` and that was clear too..

So last check was to look at `/var/log/auth` and indeed, there's a problem. Interestingly machine keys weren't generated during installation

~~~{class="bash"}
Aug 22 20:15:58 desktop sshd[1960]: fatal: No supported key exchange algorithms [preauth]
Aug 22 20:16:49 desktop sshd[1990]: error: Could not load host key: /etc/ssh/ssh_host_rsa_key
Aug 22 20:16:49 desktop sshd[1990]: error: Could not load host key: /etc/ssh/ssh_host_dsa_key
Aug 22 20:16:49 desktop sshd[1990]: error: Could not load host key: /etc/ssh/ssh_host_ecdsa_key
~~~

Ok - so the fix is easy - generate the keys with

~~~{class="bash"}
    sudo ssh-keygen -A
~~~

After that, everything's back to normal :)

### Power button shuts down computer

This is Kubuntu [Bug 1124149](https://bugs.launchpad.net/ubuntu/+source/kde-workspace/+bug/1124149). I'll spare you the details, which you can read yourself. Fix needed is to link `/usr/bin/qdbus`

~~~{class="bash"}
sudo ln -sf /usr/lib/x86_64-linux-gnu/qt4/bin/qdbus /usr/bin/qdbus
~~~


