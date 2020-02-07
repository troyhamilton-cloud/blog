---
title: "pscp and psftp are your friends"
date: 2020-01-04T07:54:43-05:00
draft: false
toc: false
images:
categories: 
  - linux 
tags:
  - pscp
  - psftp
---

As magical as Hugo is, I couldn't get the github deployment to work cleanly no matter how much I followed the official deployment guide on Windows 10 1809.  When adding the submodule for the blog repo, this error kept pestering me:
```
'public' does not have a commit checked out
```

All google-fu points to git v2.25.0.windows.1 and hugo extended v0.63.2 not being happy with each other.  Luckily I have a CentOS vm that I've been using for my RHCSA labs and deploying my hugo blog to github ran clean with no errors.  I didn't really want to retype some of the posts that I had in draft and there doesn't appear to be native clipboard support in Hyper-V.

puTTy and friends to the rescue!  Put your reading glasses on and have a gander (here)[https://www.putty.org].  If you're on Windows and install the full package, two of the hidden treasures in the puTTy box are pscp and psftp.  They do have some slight differences in syntax and I've read in some places that only psftp supports SSLv2 but after running some Wireshark and tcpdump captures it looks like both of them do.  I prefer psftp but there are use cases for both.  With either utility, I'll assume that you're already added root path of the binaries to your %PATH% environment variable,  otherwise just navigate to them.  If in doubt, echo %PATH%.

This is a very, very small and scaled down list of the commands available for both, for a comprehensive list check out (this cool guide)[https://ssh.com/ssh/putty/putty-manual/0.68/index.html] at ssh.com.  Props to the team that put together the documentation.

---
# PSFTP

* Connect to the host.  This will drop you into a **psftp>** prompt
```
psftp user@host
```

* Print remote directory
```
pwd
Current directory is /home/user
```

* Print local directory
```
lpwd
Current local directory is C:\users\user\Documents
```

* Change remote directory
```
cd /usr/local/bin
Remote directory is now /usr/local/bin
```

* List remote directory
```
ls
List direcotory /usr/local/bin
```

* Copy a file from local to remote
```
put file
local:file =>remote:/usr/local/bin/file
```

* Copy a file from remote to local
```
get file
remote:/usr/local/bin/file => local:file
```

* Exit psftp
```
quit or exit
```

# PSCP

