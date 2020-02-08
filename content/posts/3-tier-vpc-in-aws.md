---
title: "When in doubt, check your routes"
date: 2020-01-04T07:54:43-05:00
draft: false
toc: false
images:
categories:
  - cloud
tags:
  - aws
  - vpc
---

One of the cool things you can do when launching aws ec2 instances is add a configuration script that will run during launch.  From the launch configuration wizard, this script is on the 'Configure Instance Details' step under 'Advanced Details'.  A common script that I've been using lately with all of my labs is installing, starting, and enabling the apache web server aka httpd.

![user data](/images/ec2-user-data-section.png)

```
#!/bin/bash
sudo yum update -y
sudo yum install -y httpd php
sudo systemctl start httpd
sudo systemctl enable httpd
```

This is just a batch script that will update yum, install apache and php, start the httpd daemon and enable it to start on next boot.  I've probably done it a hundred times by now...

So I'm getting ready to test an application load balancer lab that I've been working on but the default apache site for my bastion host isn't working.

First let's check to make sure the service is running

```
ec2-user@ip-10-0-1-13 ~$ service httpd status
Redirecting to /bin/systemctl status httpd.service
Unit httpd.service could not be found.
```

Say what now?
But the stuff.  And the things.

The logs for the ec2 launch can be found in /var/log/cloud-init.log so let's check it out.

```
[ec2-user@ip-10-0-1-13 log]$ sudo less cloud-init-output.log

Feb 08 22:17:28 cloud-init[3070]: __init__.py[WARNING]: Unhandled non-multipart (text/x-not-multipart) userdata: 'sudo yum update -y...'
Cloud-init v. 18.5-2.amzn2 running 'modules:config' at Sat, 08 Feb 2020 22:17:29 +0000. Up 12.86 seconds.
Loaded plugins: extras_suggestions, langpacks, priorities, update-motd


 One of the configured repositories failed (Unknown),
 and yum doesn't have enough cached data to continue. At this point the only
 safe thing yum can do is fail. There are a few ways to work "fix" this:

     1. Contact the upstream for the repository and get them to fix the problem.

     2. Reconfigure the baseurl/etc. for the repository, to point to a working
        upstream. This is most often useful if you are using a newer
        distribution release than is supported by the repository (and the
        packages for the previous distribution release still work).

     3. Run the command with the repository temporarily disabled
            yum --disablerepo=<repoid> ...

     4. Disable the repository permanently, so yum won't use it by default. Yum
        will then just ignore the repository until you permanently enable it
        again or use --enablerepo for temporary usage:

            yum-config-manager --disable <repoid>
        or
            subscription-manager repos --disable=<repoid>

     5. Configure the failing repository to be skipped, if it is unavailable.
        Note that yum will try to contact the repo. when it runs most commands,
        so will have to try and fail each time (and thus. yum will be be much
        slower). If it is a very temporary problem though, this is often a nice
        compromise:

            yum-config-manager --save --setopt=<repoid>.skip_if_unavailable=true

Cannot find a valid baseurl for repo: amzn2-core/2/x86_64
Could not retrieve mirrorlist http://amazonlinux.us-east-1.amazonaws.com/2/core/latest/x86_64/mirror.list error was
12: Timeout on http://amazonlinux.us-east-1.amazonaws.com/2/core/latest/x86_64/mirror.list: (28, 'Connection timed out after 5000 milliseconds')
Feb 08 22:20:14 cloud-init[3215]: util.py[WARNING]: Package upgrade failed
Feb 08 22:20:14 cloud-init[3215]: cc_package_update_upgrade_install.py[WARNING]: 1 failed with exceptions, re-raising the last one
Feb 08 22:20:14 cloud-init[3215]: util.py[WARNING]: Running module package-update-upgrade-install (<module 'cloudinit.config.cc_package_update_upgrade_install' from '/usr/lib/python2.7/site-packages/cloudinit/config/cc_package_update_upgrade_install.pyc'>) failed
Cloud-init v. 18.5-2.amzn2 running 'modules:final' at Sat, 08 Feb 2020 22:20:15 +0000. Up 178.44 seconds.
Cloud-init v. 18.5-2.amzn2 finished at Sat, 08 Feb 2020 22:20:15 +0000. Datasource DataSourceEc2.  Up 178.62 seconds
(END)
```

Looks like yum couldn't install httpd because it couldn't find a valid baseurl?  Is yum corrupt?  Is my ami jacked?

So let's just try to install apache now.  Try being the keyword here because as it turns out we can't reach any repo or any webs.

Remember, a vpc needs an internet gateway.  An internet gateway needs a route table.  A route table needs a route, more specifically a default route.  When you create a route table the only route included is the local route for the cidr block of your vpc, well that doesn't do you any good for any hosts outside of that private subnet.  You still need a default route of 0.0.0.0/0 pointing to the internet gateway.  Attention to detail bro...

![default route](/images/vpc-route-table-default-route.png)

