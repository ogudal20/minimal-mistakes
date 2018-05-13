### Bandit 9 -> Level 10

* Task

The password for the next level is stored in the file data.txt 
in one of the few human-readable strings, beginning with several ‘=’ characters.

* Check the man pages for strings
```
strings - print the strings of printable characters in files.
```

* Seems useful
* Now we can use strings to print all human readable characters
* Then we can grep for several characters ===.

```
bandit9@bandit:~$ strings data.txt |grep "=="
```

* Now there is a list of strings with leading == characters. The last one looks the most promising.
```
========== theP`
========== password
L========== isA
========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
```

---

### Bandit10 -> Level 11

* Task

The password for the next level is stored in the file data.txt, which contains base64 encoded data

* So the password is base64 encoded.
* Look through the man pages on base64 and i recognize that -d option indicates that you can decode the base64
* I give that a try.
```
bandit10@bandit:~$ base64 -d data.txt
```
```
The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
```

---

### Bandit11 -> Level12

* Task

The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

* Overthewire provided helpful reading material in which i read through.
* Near the bottom of wiki page, i see a implementation of the rot13
```
$ # Map upper case A-Z to N-ZA-M and lower case a-z to n-za-m
$ echo "The Quick Brown Fox Jumps Over The Lazy Dog" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
Gur Dhvpx Oebja Sbk Whzcf Bire Gur Ynml Qbt
```

* So as the comment explains the all uppercase characters in the alphabet are mapped to N-Z and A-M.
* The same with lower case characters.
* I tried this example out.

```

bandit11@bandit:~$ cat data.txt |tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

* The password is revealed.

```
The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu
```

---

### Bandit12 -> Level13

* Task

The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

* So this challenge was the hardest challenge so far, and just took a long time to complete.

* First thing i did was create a new directory under tmp.
```
bandit12@bandit:~$ mkdir -p /tmp/bandit12
```

* Then copied the file in data.txt into that directory where i could play around it with it.
* Also if i made any mistakes i could always go back to /home/inhere directory and grab the file again.
```
bandit12@bandit:~$ cp data.txt /tmp/bandit12
```

* Now i change it into the directory /tmp/inhere.

```
bandit12@bandit:~$ cd /tmp/bandit12
```

* From here i had to revert the hexdump back to binary. 
* Quick man page read of xxd tells me that using the -r option you could do this.

* So revert back to binary at the same time take redirect the output to a new file.
```
bandit12@bandit:/tmp/bandit12$ xxd -r data.txt > data.bin
```
* Quick ls, and i see the new file in the directory.

```
bandit12@bandit:/tmp/bandit12$ ls -la
total 724
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 22:58 .
drwxrwx-wt 1982 root     root     724992 May 11 22:58 ..
-rw-rw-r--    1 bandit12 bandit12    618 May 11 22:58 data.bin
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
```

* Now i check the file type of the new file data.bin.]

```
bandit12@bandit:/tmp/bandit12$ file data.bin 
data.bin: gzip compressed data, was "data2.bin", last modified: Thu Dec 28 13:34:36 2017, max compression, from Unix
```

* From this output the next logically step is to decompress the gzip compressed file.
* But before we do that we need to rename the file.
* This can be done using the mv command specifying the file you want rename and then the name you want to rename it to.
```
bandit12@bandit:/tmp/bandit12$ mv data.bin data2.gz
```

* Quick ls, to see if the changes have occured.
```
bandit12@bandit:/tmp/bandit12$ ls -la
total 724
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 22:59 .
drwxrwx-wt 1982 root     root     724992 May 11 22:59 ..
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
-rw-rw-r--    1 bandit12 bandit12    618 May 11 22:58 data2.gz
```
* This confirms the new file is renamed with gzip extension.

* Now we can decompress this file and reveal its content.

```
bandit12@bandit:/tmp/bandit12$ gunzip -d data2.gz 
```
* ls to list files in the directory. 
```
bandit12@bandit:/tmp/bandit12$ ls -la
total 724
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 23:00 .
drwxrwx-wt 1982 root     root     724992 May 11 23:00 ..
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
-rw-rw-r--    1 bandit12 bandit12    585 May 11 22:58 data2
```
* We can see the file is decompress to new file called data2.
* We can see the file type by issuing the following command;
```
bandit12@bandit:/tmp/bandit12$ file data2 
data2: bzip2 compressed data, block size = 900k
```
* Another file that is compressed, this time using the bzip2 compression.
* We again rename the file to data3.bz2
```
bandit12@bandit:/tmp/bandit12$ mv data2 data3.bz2
bandit12@bandit:/tmp/bandit12$ ls -la
total 724
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 23:01 .
drwxrwx-wt 1983 root     root     724992 May 11 23:01 ..
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
-rw-rw-r--    1 bandit12 bandit12    585 May 11 22:58 data3.bz2
```
* Look at the man page on how to decompress bzip2 files.
```
bandit12@bandit:/tmp/bandit12$ bzip2 -d data3.bz2 
```
* List the files to see new file.
```
bandit12@bandit:/tmp/bandit12$ ls -la
total 724
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 23:01 .
drwxrwx-wt 1983 root     root     724992 May 11 23:01 ..
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
-rw-rw-r--    1 bandit12 bandit12    443 May 11 22:58 data3
```
* Run file, to see the file type after decompression.
```
bandit12@bandit:/tmp/bandit12$ file data3 
data3: gzip compressed data, was "data4.bin", last modified: Thu Dec 28 13:34:36 2017, max compression, from Unix
```
* Again we see gzip compression...
* Same routine;
1. rename file.
2. decompress file.
3. Check file type.

```
bandit12@bandit:/tmp/bandit12$ mv data3 data4.gz
```
```
bandit12@bandit:/tmp/bandit12$ gunzip -d data4.gz 
```
* This routine cotinues the same way for some time;

```
bandit12@bandit:/tmp/bandit12$ ls -la
total 740
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 23:02 .
drwxrwx-wt 1984 root     root     724992 May 11 23:02 ..
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
-rw-rw-r--    1 bandit12 bandit12  20480 May 11 22:58 data4
bandit12@bandit:/tmp/bandit12$ file data4
data4: POSIX tar archive (GNU)
bandit12@bandit:/tmp/bandit12$ mv data4 data5.tar
bandit12@bandit:/tmp/bandit12$ ls -la
total 740
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 23:02 .
drwxrwx-wt 1984 root     root     724992 May 11 23:02 ..
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
-rw-rw-r--    1 bandit12 bandit12  20480 May 11 22:58 data5.tar
bandit12@bandit:/tmp/bandit12$ file data5.tar 
data5.tar: POSIX tar archive (GNU)
bandit12@bandit:/tmp/bandit12$ tar -xzvf data5.tar

gzip: stdin: not in gzip format
tar: Child returned status 1
tar: Error is not recoverable: exiting now
bandit12@bandit:/tmp/bandit12$ tar -xf data5.tar 
bandit12@bandit:/tmp/bandit12$ ls -la
total 752
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 23:03 .
drwxrwx-wt 1985 root     root     724992 May 11 23:03 ..
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
-rw-r--r--    1 bandit12 bandit12  10240 Dec 28 14:34 data5.bin
-rw-rw-r--    1 bandit12 bandit12  20480 May 11 22:58 data5.tar
bandit12@bandit:/tmp/bandit12$ file data5.bin 
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/bandit12$ mv data5.bin data6.tar
bandit12@bandit:/tmp/bandit12$ ls -la
total 752
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 23:03 .
drwxrwx-wt 1986 root     root     724992 May 11 23:03 ..
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
-rw-rw-r--    1 bandit12 bandit12  20480 May 11 22:58 data5.tar
-rw-r--r--    1 bandit12 bandit12  10240 Dec 28 14:34 data6.tar
bandit12@bandit:/tmp/bandit12$ tar -xf  data6.tar 
bandit12@bandit:/tmp/bandit12$ ls -la
total 756
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 23:04 .
drwxrwx-wt 1986 root     root     724992 May 11 23:04 ..
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
-rw-rw-r--    1 bandit12 bandit12  20480 May 11 22:58 data5.tar
-rw-r--r--    1 bandit12 bandit12    226 Dec 28 14:34 data6.bin
-rw-r--r--    1 bandit12 bandit12  10240 Dec 28 14:34 data6.tar
bandit12@bandit:/tmp/bandit12$ file data6.bin 
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/bandit12$ mv data6.bin data7.bz2
bandit12@bandit:/tmp/bandit12$ ls -la
total 756
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 23:04 .
drwxrwx-wt 1986 root     root     724992 May 11 23:04 ..
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
-rw-rw-r--    1 bandit12 bandit12  20480 May 11 22:58 data5.tar
-rw-r--r--    1 bandit12 bandit12  10240 Dec 28 14:34 data6.tar
-rw-r--r--    1 bandit12 bandit12    226 Dec 28 14:34 data7.bz2
bandit12@bandit:/tmp/bandit12$ bunzip2 -d data7.bz2 
bandit12@bandit:/tmp/bandit12$ ls -la
total 764
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 23:04 .
drwxrwx-wt 1986 root     root     724992 May 11 23:04 ..
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
-rw-rw-r--    1 bandit12 bandit12  20480 May 11 22:58 data5.tar
-rw-r--r--    1 bandit12 bandit12  10240 Dec 28 14:34 data6.tar
-rw-r--r--    1 bandit12 bandit12  10240 Dec 28 14:34 data7
bandit12@bandit:/tmp/bandit12$ file data7 
data7: POSIX tar archive (GNU)
bandit12@bandit:/tmp/bandit12$ mv data7 data8.tar
bandit12@bandit:/tmp/bandit12$ ls -la
total 764
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 23:05 .
drwxrwx-wt 1986 root     root     724992 May 11 23:05 ..
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
-rw-rw-r--    1 bandit12 bandit12  20480 May 11 22:58 data5.tar
-rw-r--r--    1 bandit12 bandit12  10240 Dec 28 14:34 data6.tar
-rw-r--r--    1 bandit12 bandit12  10240 Dec 28 14:34 data8.tar
bandit12@bandit:/tmp/bandit12$ tar -xf data8.tar 
bandit12@bandit:/tmp/bandit12$ ls -la
total 768
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 23:05 .
drwxrwx-wt 1986 root     root     724992 May 11 23:05 ..
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
-rw-rw-r--    1 bandit12 bandit12  20480 May 11 22:58 data5.tar
-rw-r--r--    1 bandit12 bandit12  10240 Dec 28 14:34 data6.tar
-rw-r--r--    1 bandit12 bandit12     79 Dec 28 14:34 data8.bin
-rw-r--r--    1 bandit12 bandit12  10240 Dec 28 14:34 data8.tar
bandit12@bandit:/tmp/bandit12$ file data8.bin 
data8.bin: gzip compressed data, was "data9.bin", last modified: Thu Dec 28 13:34:36 2017, max compression, from Unix
bandit12@bandit:/tmp/bandit12$ mv data8.bin data9.gz
bandit12@bandit:/tmp/bandit12$ ls -la
total 768
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 23:05 .
drwxrwx-wt 1986 root     root     724992 May 11 23:05 ..
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
-rw-rw-r--    1 bandit12 bandit12  20480 May 11 22:58 data5.tar
-rw-r--r--    1 bandit12 bandit12  10240 Dec 28 14:34 data6.tar
-rw-r--r--    1 bandit12 bandit12  10240 Dec 28 14:34 data8.tar
-rw-r--r--    1 bandit12 bandit12     79 Dec 28 14:34 data9.gz
bandit12@bandit:/tmp/bandit12$ gunzip -d data9.gz 
bandit12@bandit:/tmp/bandit12$ ls -la
total 768
drwxrwxr-x    2 bandit12 bandit12   4096 May 11 23:06 .
drwxrwx-wt 1986 root     root     724992 May 11 23:06 ..
-rw-r-----    1 bandit12 bandit12   2646 May 11 22:58 data.txt
-rw-rw-r--    1 bandit12 bandit12  20480 May 11 22:58 data5.tar
-rw-r--r--    1 bandit12 bandit12  10240 Dec 28 14:34 data6.tar
-rw-r--r--    1 bandit12 bandit12  10240 Dec 28 14:34 data8.tar
-rw-r--r--    1 bandit12 bandit12     49 Dec 28 14:34 data9
```
* Finally we check the file on data9.
```
bandit12@bandit:/tmp/bandit12$ file data9 
data9: ASCII text
```
* display the contents of the file using cat.
```
bandit12@bandit:/tmp/bandit12$ cat data9 
The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
```

* Done.


---

### Bandit 13 -> Level 14


* Task

The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on


* So we have a private ssh key in the home directory.
* So i all i need to do is specify the private the key to login to bandit14 through ssh.
* I looked at the man page for ssh to see what option is needed to login with a private key.
```
 -i identity_file
             Selects a file from which the identity (private key) for public key authentication is read. 
```
* The following command logins in with user bandit14 to the localhost.
```
bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost
```

* Now i need to find the /etc/bandit_pass/bandit14 where the password is stored.
```
bandit14@bandit:~$ cd /etc/bandit_pass/
```
* display the contents on bandit14 password.
```
bandit14@bandit:/etc/bandit_pass$ cat bandit14
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
```

---

### Bandit14 -> Level15

* Task

The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

* I used telnet to connect to locahost on port 30000.
* Then i looked at the man pages on how to specify the a user. This is done using the -l option
* I already have the password from the last level.
```
bandit14@bandit:~$ telnet localhost 30000 -l bandit15
Trying ::1...
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
Correct!
BfMYroe26WYalil77FoDi9qh59eK5xNr

Connection closed by foreign host.
```
 
