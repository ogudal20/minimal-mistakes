Well this is first blog post, i've decided to start learning some bash. As you can see from the title i'm starting from a wargame called bandit on [OverTheWire](https://overthewire.org/)

### Bandit 0 -> Level 1

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

---  

### Bandit 1 -> Level 2

This level is still basic, just a minor hurdle;

1. We first list the files in the directory again.
2. We recognize a file -
3. Now we can't cat the file, as cat see - as stdout.
4. So we cat based on its relative path using;

```bash
#!/bin/bash

cat ./-
```

```
bandit1@bandit:~$ ls -la ./
total 24
-rw-r-----  1 bandit2 bandit1   33 Dec 28 14:34 -
drwxr-xr-x  2 root    root    4096 Dec 28 14:34 .
drwxr-xr-x 29 root    root    4096 Dec 28 14:34 ..
-rw-r--r--  1 root    root     220 Sep  1  2015 .bash_logout
-rw-r--r--  1 root    root    3771 Sep  1  2015 .bashrc
-rw-r--r--  1 root    root     655 Jun 24  2016 .profile
bandit1@bandit:~$ cat /.-
cat: /.-: No such file or directory
bandit1@bandit:~$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```

---


### Bandit 2 -> Level 3


Another straight foward level.

1. List files in the directory
2. To display files that contain a spaces in their name, you can wrap them
with quotes.
3. end.


```
bandit2@bandit:~$ ls -la
total 24
drwxr-xr-x  2 root    root    4096 Dec 28 14:34 .
drwxr-xr-x 29 root    root    4096 Dec 28 14:34 ..
-rw-r--r--  1 root    root     220 Sep  1  2015 .bash_logout
-rw-r--r--  1 root    root    3771 Sep  1  2015 .bashrc
-rw-r--r--  1 root    root     655 Jun 24  2016 .profile
-rw-r-----  1 bandit3 bandit2   33 Dec 28 14:34 spaces in this filename
bandit2@bandit:~$ cat "spaces in this filename" 
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
bandit2@bandit:~$ 
```

---


### Bandit 3 -> Level 4


* First level that the password not stored in the home directory.

1. List the files in the home directory.
2. We spot another directory name inhere
3. Change into that directory using cd
4. list files in the ~/home/inhere directory
5. file name .hidden spotted
6. cat the file.
7. end.

```
bandit3@bandit:~$ ls -la
total 24
drwxr-xr-x  3 root root 4096 Dec 28 14:34 .
drwxr-xr-x 29 root root 4096 Dec 28 14:34 ..
-rw-r--r--  1 root root  220 Sep  1  2015 .bash_logout
-rw-r--r--  1 root root 3771 Sep  1  2015 .bashrc
-rw-r--r--  1 root root  655 Jun 24  2016 .profile
drwxr-xr-x  2 root root 4096 Dec 28 14:34 inhere
bandit3@bandit:~$ cd inhere/
bandit3@bandit:~/inhere$ ls -la
total 12
drwxr-xr-x 2 root    root    4096 Dec 28 14:34 .
drwxr-xr-x 3 root    root    4096 Dec 28 14:34 ..
-rw-r----- 1 bandit4 bandit3   33 Dec 28 14:34 .hidden
bandit3@bandit:~/inhere$ cat .hidden 
pIwrPrtPN36QITSp3EQaw936yaFoFgAB
bandit3@bandit:~/inhere$ 
```

---

### Bandit 4 -> Level 5

* Task

The password for the next level is stored in the only 
human-readable file in the inhere directory.

In this task i decided to go down a different route, as i noticed by 
checking the file type of some files in the ~/inhere directory they were
labelled as data. 

So from there i created a bash script which will loop through, the
files in the directory and determine the file type of each file in this
directory.

bash script;

```bash
#!/bin/bash

#Check the file output on each file in the inhere directory.


cd ~/inhere             #Change into the correct directory
for Output in $(ls);    #Loop through the file and list them.
do
     file "./$Output"   #Determine the filetype.
done
```

* Make the file executable
* run the file

```
bandit4@bandit:/tmp/files$ chmod +x checkfile.sh 
bandit4@bandit:/tmp/files$ ./checkfile.sh 
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
bandit4@bandit:/tmp/files$ cat ~/inhere/"-file07"
koReBOKuIDDepwhWk7jZC0RTdopnAYKh
```
We can see one file has ascii text inside so i cat the file and
this reveals the password for the next level.


---

### Bandit 5 -> Level 6

* Task 
The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

    human-readable
    1033 bytes in size
    not executable

* So i first open the manual for find.
```
bandit5@bandit:~/inhere$ man find
```
* I then search on how to check the size of a file.
```
/size
```

* I find various options for size.

```
 -size n[cwbkMG]
              File uses n units of space, rounding up.  The following suffixes
              can be used:

              `b'    for  512-byte blocks (this is the default if no suffix is
                     used)

              `c'    for bytes

              `w'    for two-byte words

              `k'    for Kilobytes (units of 1024 bytes)

              `M'    for Megabytes (units of 1048576 bytes)

              `G'    for Gigabytes (units of 1073741824 bytes)
```
* So from the options it seems like we construct the following command to get
files with size of 1033 bytes.

```
find ~/inhere -size 1033c
```
* Now i need to check for files that are not executable
* Again i search for files with executable

```
/executable
```
* There is a option to specify files that are executable using the 
following option;
```
find -executable
```

* The tasks ask for file which is not executable
* In bash you can negate a command using !.
* So we can finally construct our command to find a file which 
is 1033 bytes in size and not executable.

```
bandit5@bandit:~/inhere$ find ~/inhere -size 1033c ! -executable 
```
* The output reveals a file which satisfy this requirement

```
/home/bandit5/inhere/maybehere07/.file2
```

* Cat the file to reveal the password
```
bandit5@bandit:~/inhere$ cat ~/inhere/maybehere07/.file2 
DXjZPULLxYr17uwoI01bNLQbtFemEgo7
```
 

---

### Bandit 6 -> Level 7

* Task

The password for the next level is stored somewhere on the server and has all of the following properties:

    owned by user bandit7
    owned by group bandit6
    33 bytes in size

* Go into man pages for find
* search on how to specify group name

```
 -group gname
              File belongs to group gname (numeric group ID allowed).
```
* Search on how to specify user name
```
user uname
              File is owned by user uname (numeric user ID allowed).
```

* Add the two together, we already know how to search for files based on
the size of file in bytes.

```
find / -user bandit7 -group bandit6 -size 33c
```

* File that matches these properties
```
/var/lib/dpkg/info/bandit7.password
```

* cat the file and password is revealed.
```
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
```

---

### Bandit 7 -> Level 8

* Task

The password for the next level is stored in the file data.txt 
next to the word millionth.

* This challenge i will be using grep, to find the word millionth
```
bandit7@bandit:~$ grep "millionth" data.txt 
```

* This reveals the password for the next level.

```
millionth	cvX2JJa4CFALtqS87jk27qwqGhBM9plV
```

---

### Bandit 8 -> Level 9

* Task 
The password for the next level is stored in the file data.txt and is the only line of text that occurs only once


* I first looked at the man page for uniq
``` 
bandit8@bandit:~$ man uniq 
```

* The command essential removes duplicate lines, counts them etc.
```
 uniq - report or omit repeated lines
```

* There is option which you can only print unique line. So i first try
this command out.

```
bandit8@bandit:~$ uniq -u data.txt 
```

* Unfortunately it display all the strings in the file.

* So i then decided to check the man page for sort.

* Which just sorts strings in alphabetical order.

* From there i first sorted the strings in the file then piped the stdout to
stdin for uniq strings in the file with the -u option.

```
bandit8@bandit:~$ sort data.txt |uniq -u
```

* This command gave the password for the next level.
```
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
```

