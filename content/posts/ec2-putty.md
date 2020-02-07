---
title: "Fun with AWS EC2 and puTTy"
date: 2020-01-02T13:52:16-05:00
draft: false
toc: false
images:
categories: 
  - cloud
tags:
  - aws
  - ec2
---


In preparation for SAA-C01 I wanted to start looking into moving my site from Github+Netlify to EC2+S3+Jenkins.  So I go to ssh into my free-tier instance from PuTTy on win10 and I get:

```
No supported authentication methods available (server sent: publickey,gssapi-keyex,gssapi-with-mic
```

Interesting, but ok, let's see what's wrong.  So I login to aws console and verify my ec2 instance is running.  The green dot means running right :).  But we're architects/engineers/sysops/devops so we do things in cli right?

```
aws ec2 describe-instance-status
```

```json
{     "InstanceStatuses": [
        {
            "AvailabilityZone": "us-east-1c",
            "InstanceId": "i-0x0x0x0x0x0x",
            "InstanceState": {
                "Code": 16,
                "Name": "running"
            },
            "InstanceStatus": {
                "Details": [                     {
                        "Name": "reachability",
                        "Status": "passed"
                    }
                ],                 "Status": "ok"
            },
            "SystemStatus": {
                "Details": [                     {
                        "Name": "reachability",
                        "Status": "passed"
                    }
                ],                 "Status": "ok"
            }
        }
    ]
}
```

Looks right to me.  Let's verify the security group after checking our local public ip using this powershell script:

```
Invoke-RestMethod https://ipinfo.io/ip
````

First check what security group is applied to the instance:

```
aws ec2 describe-instances
```

```json
"SecurityGroups": [ 		{
			"GroupName": "launch-wizard-2",
			"GroupId": "sg-0x0x0x0x0x0x0x"
		}
	]
```

Cool, let's check out the security group inbound rules:

```
aws ec2 describe-security-groups
```

```json
{ 	"FromPort": 22,
	"IpProtocol": "tcp",
	"IpRanges": [ 		{
			"CidrIp": "my.public.ip.here/32"
		}
	], 	"Ipv6Ranges": [],
	"PrefixListIds": [],
	"ToPort": 22,
	"UserIdGroupPairs": []
}
```

Looks right to me there too.  So wtf?  Since this isn't a production box and I don't have anything critical on it, let's just reboot the box.  I know, I know, this shouldn't be the answer but for the sake of learning that's what we're going with.

```
aws ec2 stop-instances --instance-ids i-0x0x0x0x0x
````

```
aws ec2 start-instances --instance-ids i-0x0x0x0x0x
```

After the box boots up, let's try to ssh again... but wait!  Remember, ALWAYS CHECK THE PHYSICAL LAYER!!!  I load the session in putty and what do I see?  ec2-user;@publicdns.  A freaking semicolon after ec2-user!  Seriously I have no idea how that got there.  I delete the semicolon and I ssh to the instance right away.  Gggrrr!

Long story short, always check the physical layer.  I use the term physical layer loosely, in this scenario it's putty and the session hostname.
