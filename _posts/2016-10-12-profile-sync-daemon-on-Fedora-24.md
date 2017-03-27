---
published: true
layout: post
title: "profile-sync-daemon on Fedora 24"
author: MochaPenguin
summary: PSD is designed to manage browser profile(s) in tmpfs and to periodically sync back to the physical disc, thereby reducing writes to SSDs. This is useful where there is ample spare RAM on the system.
categories: [Howto, Articles]
tags:
- profile-sync-daemon
---

* Table of Contents
{:toc}

# Background

PSD is designed to manage browser profile(s) in tmpfs and to
  periodically sync back to the physical disc, thereby reducing writes
  to SSDs. This is useful where there is ample spare RAM on the
  system.

# Installing profile-sync-daemon

<p>profile-sync-daemon was not available in Fedora repositories at
  the time. To build and install it</p>

<p>
  Download the source from <a
    href="https://github.com/graysky2/profile-sync-daemon">https://github.com/graysky2/profile-sync-daemon</a>
</p>

```bash
[mochapenguin@dv6tqe psd]su -
[root@dv6tqe psd]# git clone https://github.com/graysky2/profile-sync-daemon.git
[root@dv6tqe psd]# cd profile-sync-daemon/
[root@dv6tqe profile-sync-daemon]# make
Setting version
[root@dv6tqe profile-sync-daemon]# make install
Installing main script...
install -p -d "/usr/bin"
install -p -m755 common/profile-sync-daemon "/usr/bin/profile-sync-daemon"
install -p -m755 common/psd-overlay-helper "/usr/bin/psd-overlay-helper"
ln -s profile-sync-daemon "/usr/bin/psd"
install -p -d "/usr/share/zsh/site-functions"
install -p -m644 common/zsh-completion "//usr/share/zsh/site-functions/_psd"
install -p -d "/usr/share/psd/browsers"
install -p -m644 common/psd.conf "/usr/share/psd/psd.conf"
cp common/browsers/* "/usr/share/psd/browsers"
Installing manpage...
install -p -d "/usr/share/man/man1"
install -p -m644 doc/psd.1 "/usr/share/man/man1/psd.1"
install -p -m644 doc/psd-overlay-helper.1 "/usr/share/man/man1/psd-overlay-helper.1"
gzip -9 "/usr/share/man/man1/psd.1"
gzip -9 "/usr/share/man/man1/psd-overlay-helper.1"
ln -s psd.1.gz "/usr/share/man/man1/profile-sync-daemon.1.gz"
Installing systemd files...
install -p -d "/usr/lib/systemd/user"
install -p -m644 init/psd.service "/usr/lib/systemd/user/psd.service"
install -p -m644 init/psd-resync.service "/usr/lib/systemd/user/psd-resync.service"
install -p -m644 init/psd-resync.timer "/usr/lib/systemd/user/psd-resync.timer"    
```


## First run

When profile-sync-daemon is run for the first time, it outputs
  the following:

~~~
[mochapenguin@dv6tqe ~]$ psd p
First time running psd so please edit /home/mochapenguin/.config/psd/psd.conf to your liking and run again.</pre>
~~~

Create the config file.

~~~
[mochapenguin@dv6tqe ~]$ psd p
Profile-sync-daemon v6.25 on Fedora 24 (Workstation Edition)

Systemd service is currently active.
Systemd resync-timer is currently active.
Overlayfs v23 is currently active.

Psd will manage the following per /home/mochapenguin/.config/psd/psd.conf:

browser/psname:  firefox/firefox
owner/group id:  mochapenguin/1000
sync target:     /home/mochapenguin/.mozilla/firefox/fu8u7xk0.default
tmpfs dir:       /run/user/1000/mochapenguin-firefox-fu8u7xk0.default
profile size:    14M
overlayfs size:  
recovery dirs:   none

browser/psname:  google-chrome/chrome
owner/group id:  mochapenguin/1000
sync target:     /home/mochapenguin/.config/google-chrome
tmpfs dir:       /run/user/1000/mochapenguin-google-chrome
profile size:    5.8M
overlayfs size:  
recovery dirs:   none</pre>
~~~

# Usage

More information on profile-sync-daemon usage can be found on <a
    href="https://wiki.archlinux.org/index.php/Profile-sync-daemon#Using_psd">Arch
    Linux Profile Sync Daemon page</a>
