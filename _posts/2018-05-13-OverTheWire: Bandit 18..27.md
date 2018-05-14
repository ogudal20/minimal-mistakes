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


