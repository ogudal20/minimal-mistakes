Well this is first blog post, i've decided to start learning some bash. As you can see from the title i'm starting from a wargame called bandit on https://overthewire.org/

### Bandit 0
Pretty straightfoward level;
1. List the files in the directory
2. display the contents of the readme file

```
bandit0@bandit:~$ ls -la
total 24
drwxr-xr-x  2 root    root    4096 Dec 28 14:34 .
drwxr-xr-x 29 root    root    4096 Dec 28 14:34 ..
-rw-r--r--  1 root    root     220 Sep  1  2015 .bash_logout
-rw-r--r--  1 root    root    3771 Sep  1  2015 .bashrc
-rw-r--r--  1 root    root     655 Jun 24  2016 .profile
-rw-r-----  1 bandit1 bandit0   33 Dec 28 14:34 readme
bandit0@bandit:~$ cat readme 
boJ9jbbUNNfktd78OOpsqOltutMc3MY1
bandit0@bandit:~$ 
```
