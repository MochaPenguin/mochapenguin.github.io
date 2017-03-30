---
published: true
layout: post
title: Install fun_plug (0.7) on D-Link DNS-315 (NAS)
excerpt: fun_plug on DNS-315
categories: Articles
tags:
  - fun_plug
  - DNS-315
comments: true
---

* Table of Contents
{:toc}

# Introduction

These are my notes on installing and configuring fun_plug (0.7) on D-Link DNS-315 (NAS). If you come across anything that doesn't come across clear please leave a comment and I shall attempt to add more explanation.

For me the primary motive for installing fun_plug was to enable SSH. This allows me to mount directories on my NAS onto my laptop / desktop.

# Resources

- [NAS-TWEAKS.NET article](http://nas-tweaks.net/371/hdd-installation-of-the-fun_plug-0-7-on-nas-devices/)

# Pre-requisites

## Remote Backup

Please ensure that the internal "Remote Backup" Services is deactivated. This server is shipped with newer NAS like the **DNS-320/DNS-320L/DNS-325/DNS-345** and conflicts with the SSH-Server of the fun_plug.


# Installation and configuration

## Overview of Installation steps

- Follow the steps [here](http://nas-tweaks.net/371/hdd-installation-of-the-fun_plug-0-7-on-nas-devices/#Steps_for_installing_fun_plug). Download [ARM version of fun_plug.tgz](http://inreto.de/dns323/fun-plug/0.7/arm/fun_plug.tgz) and copy the 2 files to root of drive
- Fix script issues
  - change permissions of script - executable (on Linux)
  - the HDD mount point is /mnt/HD/HD_a2
- Reboot
- fun_plug script should run and install fun_plug
- change root password - described [here](http://nas-tweaks.net/371/hdd-installation-of-the-fun_plug-0-7-on-nas-devices/#Change_root_password)
- Apply the steps described in the post-install fixes section
- activate services by chmod a+x the respective scripts
  - For instance, to enable ssh daemon, type chmod a+x /ffp/startup/sshd.sh
- [if you wish to use SSH] See the section on SSH Configuration for additional configuration

## Post install fixes

### permission issues

There appears to be a problem with file permissions – after reboot most of them are reset to 777 and that upsets sshd and also prevents choosing the startup scripts.

To address this, modify  **/ffp/etc/fun_plug.init**

I added to /ffp/etc/fun_plug.init the following line, just before the section dedicated to enable/disable services:

/ffp/etc/fun_plug.init
```bash
chmod 600 /ffp/etc/ssh/ssh_host_*
chmod 755 /ffp/var/lib/sshd
```
### fixes for sshd.sh

* I also changed a line in /ffp/start/sshd.sh to correctly check for the /ffp/etc/ssh/ssh_host_ecdsa_key file.

http://nas-tweaks.net/371/hdd-installation-of-the-fun_plug-0-7-on-nas-devices/#comment-22484

It appears this error may be to a invalid reference to the ssh file in **/ffp/start/sshd.sh** script.

/ffp/start/sshd.sh
```bash
The original script reads:

if [ ! -f /etc/ssh/ssh_host_ecdsa_key ]; then
/ffp/bin/ssh-keygen -t ecdsa -f /ffp/etc/ssh/ssh_host_ecdsa_key -N ''
fi

I believe it should read:

if [ ! -f /ffp/etc/ssh/ssh_host_ecdsa_key ]; then
/ffp/bin/ssh-keygen -t ecdsa -f /ffp/etc/ssh/ssh_host_ecdsa_key -N ''
fi
#!/ffp/bin/sh
```

Set up rsync option.

@see troubleshooting section

#### Adding users

added user using the web interface

#### Configuring User Home directories

```bash
usermod -s /ffp/bin/sh mochapenguin
mkdir -p /ffp/home/mochapenguin/
sed -ie 's#:/usr/share/ftp/mochapenguin:#:/ffp/home/mochapenguin:#g' /etc/passwd
pwconv

usermod -d /ffp/home/mochapenguin/ mochapenguin
store-passwd.sh # [saves changes to disk]
```

_Yay!! It worked!!_

#### User and user .ssh folder permissions

Once the users have been created, set the permissions of the `user` and user's `.ssh` folder at boot.

Create a script to put back the permissions of the user and user .ssh folder at boot

Reference: [nas-tweaks](http://nas-tweaks.net/371/hdd-installation-of-the-fun_plug-0-7-on-nas-devices/#Option:_view_the_fun_plug_script)

log in as root (did it using a telnet session)
```bash
touch /ffp/etc/fun_plug.local
```

/ffp/etc/fun_plug.local
```bash
#!/ffp/bin/sh
chmod 700 /ffp/home/mochapenguin
chmod 600 /ffp/home/mochapenguin/.ssh/*
chmod 700 /ffp/home/mochapenguin/.ssh
```

### SSH Configuration
#### Using certificates

To have a better understanding of the concept of private, public key authentication read [this nas-tweaks post](http://nas-tweaks.net/53/private-public-key-authentication-with-openssh-in-ffp/)

##### Changing the home of the user

By default, the home directory of the user **root** is set to `/home`, which is located in the internal flash-memory. This memory is deleted every time the device is rebooted. Thus the home-directory of the each user should be moved to be a sub-directory of ffp.

```bash
# Create directory for new home for root
mkdir -p /ffp/home/root/
# Modify user
usermod -d /ffp/home/root/ root
store-passwd.sh
```

You need to repeat the last two steps every time your firmware gets upgraded.

##### Generating the keypair

```bash
mkdir /ffp/home/root/.ssh
cd /ffp/home/root/.ssh
```

There are now two choices for the key. You can either have a password (which can automatically be entered by ssh-agent or PuttY Pageant) or no password. Both ways are described below and will generate two files named keypair and keypair.pub which contain the private and public key needed for the authentication:

- Without Password
`ssh-keygen -f keypair -C 'Generated by nas-tweaks.net' -t rsa -q`

_OR_

- With Password
`ssh-keygen -f keypair -C 'Generated by nas-tweaks.net' -N '' -t rsa -q`

##### Adding public key

create .ssh folder in /ffp/home/mochapenguin as user mochapenguin

- change permission of directory to `700`
- copy the pub file to .ssh folder
- set chmod to `600`

##### Activating public key authentication in SSH
Open **/ffp/etc/ssh/sshd_config** and search for the following line:
```bash
#PubkeyAuthentication yes
```

And remove the comment:

```bash
PubkeyAuthentication yes
```

##### Moving the generated keys to authorized_keys

```bash
touch authorized_keys
chmod 600 authorized_keys
cat keypair.pub >> authorized_keys
```

restart SSH service

```bash
root@DNS-315:~# sh /mnt/HD/HD_a2/ffp/start/sshd.sh restart
```

# Log output

Here is the fun_plug startup log from DNS-315. There are a couple of things I am not happy about. But they are not show stoppers anyway. I shall be looking into them as soon as I get time. Any suggestions in the meantime are welcome.

ffp.log
~~~
**** fun_plug script for DNS-323 (2008-08-11 tp@fonz.de) ****
**** fun_plug script modded by Uli (2012-02-21 ffp@wolf-u.li) ****
Sat Sep 14 01:35:28 BST 2013
ln -snf /mnt/HD/HD_a2/ffp /ffp
* Running /ffp/etc/fun_plug.init ...
* Running /ffp/etc/fun_plug.local ...
* Running /ffp/etc/rc ...
* /ffp/start/SERVERS.sh ...
* /ffp/start/portmap.sh ...
Starting /ffp/sbin/rpc.portmap
* /ffp/start/nfsd.sh ...
Mounting /proc/fs/nfsd ...
WARNING: rpc.portmap: Already running <== How do I turn off rpc portmap?
Starting /ffp/sbin/rpc.statd
Starting /ffp/sbin/rpc.nfsd 8
rpc.nfsd: unable to resolve ANYADDR:nfs to inet address: Servname not supported for ai_socktype <= WTF?
rpc.nfsd: unable to set any sockets for nfsd
Starting /ffp/sbin/rpc.mountd
* /ffp/start/LOGIN.sh ...
* /ffp/start/telnetd.sh ...
Starting /ffp/sbin/telnetd -l /ffp/bin/sh
* /ffp/start/sshd.sh ...
Starting /ffp/sbin/sshd
* /ffp/start/rsyncd.sh ...
Error: Missing config file /ffp/etc/rsyncd.conf
* /ffp/start/kickwebs.sh ...
Kicking webs ...
webs: no process found
* /ffp/start/lighttpd.sh ... <= Should I turn this off for now? How?
/ffp/etc/lighttpd.conf: Required file not found or not readable
* /ffp/start/inetd.sh ...
/ffp/etc/inetd.conf: Required file not found or not readable
* /ffp/start/fp_master.sh ...
Starting /ffp/sbin/fp.master
Ready.
~~~

# Troubleshooting
