# simplectf tryhackme

> ip = 10.10.165.13

1. nmap scan 
	> got this
	```
	Starting Nmap 7.80 ( https://nmap.org ) at 2021-05-03 15:54 IST
	Stats: 0:00:49 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
	NSE Timing: About 99.75% done; ETC: 15:55 (0:00:00 remaining)
	Nmap scan report for 10.10.165.13
	Host is up (0.19s latency).
	Not shown: 997 filtered ports
	PORT     STATE SERVICE VERSION
	21/tcp   open  ftp     vsftpd 3.0.3
	| ftp-anon: Anonymous FTP login allowed (FTP code 230)
	|_Can't get directory listing: TIMEOUT
	| ftp-syst: 
	|   STAT: 
	| FTP server status:
	|      Connected to ::ffff:10.9.5.183
	|      Logged in as ftp
	|      TYPE: ASCII
	|      No session bandwidth limit
	|      Session timeout in seconds is 300
	|      Control connection is plain text
	|      Data connections will be plain text
	|      At session startup, client count was 1
	|      vsFTPd 3.0.3 - secure, fast, stable
	|_End of status
	80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
	| http-robots.txt: 2 disallowed entries 
	|_/ /openemr-5_0_1_3 
	|_http-server-header: Apache/2.4.18 (Ubuntu)
	|_http-title: Apache2 Ubuntu Default Page: It works
	2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
	| ssh-hostkey: 
	|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
	|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
	|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)
	Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
	
	Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
	```

2. trying to gain acses in ftp server using ncarck and '/usr/share/seclists/Passwords/Common-Credentials/best110.txt' wordlist | failed

3. I tried to use nmap scripts to find vuln but it does not have the scripts which i wanted
	```
	nmap --script=ftp* 10.10.165.13
	zsh: no matches found: --script=ftp*
	```

so i used this 
	```
	nmap --script=ftp* 10.10.165.13
	Nmap scan report for 10.10.165.13
	Host is up (0.20s latency).
	Not shown: 997 filtered ports
	PORT     STATE SERVICE
	21/tcp   open  ftp
	|_sslv2-drown: 
	80/tcp   open  http
	|_http-csrf: Couldn't find any CSRF vulnerabilities.
	|_http-dombased-xss: Couldn't find any DOM based XSS.
	| http-enum: 
	|_  /robots.txt: Robots file
	| http-slowloris-check: 
	|   VULNERABLE:
	|   Slowloris DOS attack
	|     State: LIKELY VULNERABLE
	|     IDs:  CVE:CVE-2007-6750
	|       Slowloris tries to keep many connections to the target web server open and hold
	|       them open as long as possible.  It accomplishes this by opening connections to
	|       the target web server and sending a partial request. By doing so, it starves
	|       the http server's resources causing Denial Of Service.
	|       
	|     Disclosure date: 2009-09-17
	|     References:
	|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2007-6750
	|_      http://ha.ckers.org/slowloris/
	|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
	2222/tcp open  EtherNetIP-1
	
	Nmap done: 1 IP address (1 host up) scanned in 338.84 seconds


	```

4. i tired to check the robots.txt and got this
	```
	#
	# "$Id: robots.txt 3494 2003-03-19 15:37:44Z mike $"
	#
	#   This file tells search engines not to index your CUPS server.
	#
	#   Copyright 1993-2003 by Easy Software Products.
	#
	#   These coded instructions, statements, and computer programs are the
	#   property of Easy Software Products and are protected by Federal
	#   copyright law.  Distribution and use rights are outlined in the file
	#   "LICENSE.txt" which should have been included with this file.  If this
	#   file is missing or damaged please contact Easy Software Products
	#   at:
	#
	#       Attn: CUPS Licensing Information
	#       Easy Software Products
	#       44141 Airport View Drive, Suite 204
	#       Hollywood, Maryland 20636-3111 USA
	#
	#       Voice: (301) 373-9600
	#       EMail: cups-info@cups.org
	#         WWW: http://www.cups.org
	#
	User-agent: *
	Disallow: /
	
	
	Disallow: /openemr-5_0_1_3 
	#
	# End of "$Id: robots.txt 3494 2003-03-19 15:37:44Z mike $".
	#

	```
	
5. i saw some walkthrogh and saw gobuster so lets use it
	```
	gobuster dir -u http://10.10.165.13/ -w /usr/share/dirb/wordlists/common.txt

	```
	and found a '/simple' path 'http://10.10.165.13/simple/'
6. trying to exploit this 'http://10.10.165.13/simple/' site because it is vulnerrable to sal iject after googling is found this script 
'https://www.exploit-db.com/exploits/46635' not lets use it
	```
	python2 46635.py -u http://10.10.165.13/simple/ --crack -w /usr/share/wordlists/rockyou.txt
	
	
	```
user: mitch
password: 0c01f44468b2




