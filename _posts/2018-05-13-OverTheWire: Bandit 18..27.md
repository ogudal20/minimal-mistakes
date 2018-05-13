### Bandit18 -> Level19

* Task

The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

* This exits with bye message and closes the ssh connection.
* We can run commands while at the same time connecting through ssh on the host.
```
root@kali:~# ssh bandit18@bandit.labs.overthewire.org -p 2220 cat /etc/bandit_pass/bandit18
```

* This reveals the password.
```
kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
```

