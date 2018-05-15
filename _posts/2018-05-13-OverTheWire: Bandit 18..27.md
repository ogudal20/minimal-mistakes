### Bandit18 -> Level19

* Task

The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

* This exits with bye message and closes the ssh connection.
* We can run commands while at the same time connecting through ssh on the host.
* So i first connect to the host through ssh and list the files in the directory.

```
root@kali:~# ssh bandit18@bandit.labs.overthewire.org -p 2220 ls -la
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames
bandit18@bandit.labs.overthewire.org's password: 
total 24
drwxr-xr-x  2 root     root     4096 Dec 28 14:34 .
drwxr-xr-x 29 root     root     4096 Dec 28 14:34 ..
-rw-r--r--  1 root     root      220 Sep  1  2015 .bash_logout
-rw-r-----  1 bandit19 bandit18 3794 Dec 28 14:34 .bashrc
-rw-r--r--  1 root     root      655 Jun 24  2016 .profile
-rw-r-----  1 bandit19 bandit18   33 Dec 28 14:34 readme
```
* We can see a readme file in the home directory
* i cat the file. 

```
root@kali:~# ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames
bandit18@bandit.labs.overthewire.org's password: 
IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x

```

* The password.

```
IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x
```

---

### Bandit19 -> Level20

* Task
To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

* This task had file which had evalated permission. There is a stickybit.

```
bandit19@bandit:~$ ls -la
total 28
drwxr-xr-x  2 root     root     4096 Dec 28 14:34 .
drwxr-xr-x 29 root     root     4096 Dec 28 14:34 ..
-rw-r--r--  1 root     root      220 Sep  1  2015 .bash_logout
-rw-r--r--  1 root     root     3771 Sep  1  2015 .bashrc
-rw-r--r--  1 root     root      655 Jun 24  2016 .profile
-rwsr-x---  1 bandit20 bandit19 7408 Dec 28 14:34 bandit20-do
```
* setuid stands for set user ID on execution. This allows users to run certain programs with escalated privileges in this case bandit19.

* I this situation i executed the program bandit20-do whilst displaying the contents of /etc/bandit_pass/bandit20.

```
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
```

* This gave me the password for the next level.


---

### Bandit20 -> Level21

* Task
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

* The most important tool in this task was netcat.
* So it is important to read the man pages on this tool.

* I first created a listener with netcat.On port 6969.

```
bandit20@bandit:~$ nc -lv 6969
```

* -l option is to listen on a port.
* -v option is for verbose.

* I then open up another terminal where i executed the program suconnect. On port 6969.

```
bandit20@bandit:~$ ./suconnect 6969
```

* I pasted in the password from the previous level.

```
bandit20@bandit:~$ ./suconnect 6969
Read: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
Password matches, sending next password
```
* Recieved a password matches.
* Went back to my other terminal where the listener is running.

```
bandit20@bandit:~$ nc -lvp 6969
Listening on [0.0.0.0] (family 0, port 6969)
Connection from [127.0.0.1] port 6969 [tcp/*] accepted (family 2, sport 54132)
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr
```

* As you can see it echoed back the password for the next level.


---

### Bandit21 -> Level22

* Task

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

* First step i took was to look into the /etc/cron.d/ directory.

```
bandit21@bandit:~$ cd /etc/cron.d
```

* List the files in the directory.

```
bandit21@bandit:/etc/cron.d$ ls -la
total 28
drwxr-xr-x   2 root root 4096 Dec 28 14:34 .
drwxr-xr-x 100 root root 4096 Mar 12 09:51 ..
-rw-r--r--   1 root root  102 Apr  5  2016 .placeholder
-rw-r--r--   1 root root  120 Dec 28 14:34 cronjob_bandit22
-rw-r--r--   1 root root  122 Dec 28 14:34 cronjob_bandit23
-rw-r--r--   1 root root  120 Dec 28 14:34 cronjob_bandit24
-rw-r--r--   1 root root  190 Oct 31  2017 popularity-contest
```

* The cronjob_bandit22 file look interesting. So i looked into the contents for the file.

```
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

* So it looks like a shell script is run every minute.
* I searched for the file and look into the contents.

```
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh 
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
* From here we can see a the password for the next level is redirected to the /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv. 

* I displayed the contents of this file.

```
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI
```

* This gave me the password for the next level.


