---
layout: single
title: A Pivot CTF Writeup
tags: pivot
date: 2019-12-23
classes: wide
excerpt: Pivoting refers to a method used by penetration testers that uses the compromised system to attack other systems on the same network to avoid restrictions such as firewall configurations, which may prohibit direct access to all machines.
---
Pivoting refers to a method used by penetration testers that uses the compromised system to attack other systems on the same network to avoid restrictions such as firewall configurations, which may prohibit direct access to all machines. 

## Details
You will get access to a machine A 

Ip : 172.19.0.2

![](/assets/images/PivotCTF/screenshot1.png)

Machine B

Ip: 172.19.0.3

Machine B has another subnet 192.168.0.1/24

![](/assets/images/PivotCTF/screenshot2.png)


Your target is Machine C 

Ip: 192.168.0.2 which can not be accessable by Machine A

Only Machine B can access it 



Your task is to ssh to Machine C from Machine A 

Use pivoting for the purpose and all the credentials are same everywhere

But login as att user when ssh because ssh to direct root user is closed 


Happy learning

## Solution:

1. Connect to the first machine which is machine A with the IP of `172.19.0.2`


2. Setup a Dynamic Port Forwarding:
	```
	Dynamic Port Forwarding allows a communication not on a single port, 
	but across a range of ports. 
	Which means that you can set a port in your local machine 
	to receive and send connections from different ports of the host you are connecting to. 
	```
	
	```
	ssh -D 9050 att@172.19.0.3
	```

3. Setup your proxychains with the same option below:

	![78ac1965.png](/assets/images/PivotCTF/78ac1965.png)

4. Run ssh with proxychains:
	```
	$ proxychains ssh att@192.168.0.2
	```
	![](/assets/images/PivotCTF/screenshot3.png)
5. Get Flag
	```
	att@98c2a9c771ec:~$ ls
	flag.txt
	att@98c2a9c771ec:~$ cat flag.txt 
	Stephen is gay
	att@98c2a9c771ec:~$ 
	```



