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
 
---

###Bandit15 -> Level16

* Task

The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.

* I looked at the man pages for s_client on how to connect to to host and port
```
[-connect host:port]
```
* Now i need to add the -ign_eof which stops the connection shutting down when the end of file is reached.

```
bandit14@bandit:~$ openssl s_client -connect localhost:30001 -ign_eof 
```

* Output
```
CONNECTED(00000003)
depth=0 CN = bandit
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = bandit
verify return:1
---
Certificate chain
 0 s:/CN=bandit
   i:/CN=bandit
---
Server certificate
-----BEGIN CERTIFICATE-----
MIICsjCCAZqgAwIBAgIJAKZI1xYeoXFuMA0GCSqGSIb3DQEBCwUAMBExDzANBgNV
BAMMBmJhbmRpdDAeFw0xNzEyMjgxMzIzNDBaFw0yNzEyMjYxMzIzNDBaMBExDzAN
BgNVBAMMBmJhbmRpdDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAOcX
ruVcnQUBeHJeNpSYayQExCJmcHzSCktnOnF/H4efWzxvLRWt5z4gYaKvTC9ixLrb
K7a255GEaUbP/NVFpB/sn56uJc1ijz8u0hWQ3DwVe5ZrHUkNzAuvC2OeQgh2HanV
5LwB1nmRZn90PG1puKxktMjXsGY7f9Yvx1/yVnZqu2Ev2uDA0RXij/T+hEqgDMI7
y4ZFmuYD8z4b2kAUwj7RHh9LUKXKQlO+Pn8hchdR/4IK+Xc4+GFOin0XdQdUJaBD
8quOUma424ejF5aB6QCSE82MmHlLBO2tzC9yKv8L8w+fUeQFECH1WfPC56GcAq3U
IvgdjGrU/7EKN5XkONcCAwEAAaMNMAswCQYDVR0TBAIwADANBgkqhkiG9w0BAQsF
AAOCAQEAnrOty7WAOpDGhuu0V8FqPoKNwFrqGuQCTeqhQ9LP0bFNhuH34pZ0JFsH
L+Y/q4Um7+66mNJUFpMDykm51xLY2Y4oDNCzugy+fm5Q0EWKRwrq+hIM+5hs0RdC
nARP+719ddmUiXF7r7IVP2gK+xqpa8+YcYnLuoXEtpKkrrQCCUiqabltU5yRMR77
3wqB54txrB4IhwnXqpO23kTuRNrkG+JqDUkaVpvct+FAdT3PODMONP/oHII3SH9i
ar/rI9k+4hjlg4NqOoduxX9M+iLJ0Zgj6HAg3EQVn4NHsgmuTgmknbhqTU3o4IwB
XFnxdxVy0ImGYtvmnZDQCGivDok6jA==
-----END CERTIFICATE-----
subject=/CN=bandit
issuer=/CN=bandit
---
No client certificate CA names sent
---
SSL handshake has read 1015 bytes and written 631 bytes
---
New, TLSv1/SSLv3, Cipher is AES128-SHA
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1
    Cipher    : AES128-SHA
    Session-ID: D7F4D507E5A761968BC75EBA247F6ACE8FB93DA72C32DA99AD2F2AFBC115203F
    Session-ID-ctx: 
    Master-Key: 2EA72FA70D41AF9C7C8F2E1C22B7D7CC4CEDD1115BDA15E6657EBF7FEE8BD5E930760C91EA576CECEE0B225B709CF548
    Key-Arg   : None
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - 08 f0 15 a5 d6 6f a0 e8-06 d6 bb a4 0c 33 eb 04   .....o.......3..
    0010 - c7 db aa 5c ad b9 2b 08-2d a5 47 a6 b5 48 e5 c9   ...\..+.-.G..H..
    0020 - 6c 95 83 af b2 2c cb d2-51 38 cb 10 77 61 2b 9d   l....,..Q8..wa+.
    0030 - 31 83 a4 74 24 a3 35 ee-da 85 4b c8 25 5c 43 81   1..t$.5...K.%\C.
    0040 - 44 9b d9 fe 35 fd c4 90-3a ac 49 e0 fd 06 b0 28   D...5...:.I....(
    0050 - be d9 43 74 a5 2b e6 b3-8b ca c4 ec b3 5b 03 85   ..Ct.+.......[..
    0060 - f5 ee 57 78 0a 5f 08 d9-25 c3 42 cb de 56 69 53   ..Wx._..%.B..ViS
    0070 - 0b 86 33 2d 24 a0 0e 80-f3 fc 47 f8 39 71 83 70   ..3-$.....G.9q.p
    0080 - 14 a1 fa f7 d7 52 4a 5a-ea 29 9c e8 7e 90 79 1c   .....RJZ.)..~.y.
    0090 - 31 75 89 90 e2 51 b3 f3-3f 8f a1 69 c7 5b 61 f7   1u...Q..?..i.[a.

    Start Time: 1526211835
    Timeout   : 300 (sec)
    Verify return code: 18 (self signed certificate)
---
BfMYroe26WYalil77FoDi9qh59eK5xNr
Correct!
cluFn7wTiGryunymYOu4RcffSxQluehd

closed
```
* Near the end at pasted in the password for the last level and that echo back the level for the next level.


---

### Bandit16 -> Level17

* Task

The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

* In this challenge i had to use nmap to portscan on a range of ports
* I already have some previous experience with the tool so this was not a difficult challenge.
```
bandit16@bandit:~$ nmap -A -vv -sT -T4 -p31000-32000 localhost
```
* Option -A is to enable OS and version detection.
* Option -vv is for verbose. Just get info on the nmap while its running.
* Option -sT is to connect to tcp socket.
* Option -T4 for a more agressive scan. Faster as well.
* Option -p31000-32000 to scan between the port ranges 31000-32000
* localhost the host we are scanning.

* The output revealed.
```
PORT      STATE SERVICE     REASON  VERSION
31046/tcp open  echo        syn-ack
31518/tcp open  ssl/echo    syn-ack
| ssl-cert: Subject: commonName=bandit
| Issuer: commonName=bandit
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2017-12-28T13:23:40
| Not valid after:  2027-12-26T13:23:40
| MD5:   9ceb 80ed 781c 17a4 55d0 5e61 b001 0ebe
| SHA-1: ab78 886c 4527 c26e 6a4c 7808 aa2d 93d1 5512 e33f
| -----BEGIN CERTIFICATE-----
| MIICsjCCAZqgAwIBAgIJAKZI1xYeoXFuMA0GCSqGSIb3DQEBCwUAMBExDzANBgNV
| BAMMBmJhbmRpdDAeFw0xNzEyMjgxMzIzNDBaFw0yNzEyMjYxMzIzNDBaMBExDzAN
| BgNVBAMMBmJhbmRpdDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAOcX
| ruVcnQUBeHJeNpSYayQExCJmcHzSCktnOnF/H4efWzxvLRWt5z4gYaKvTC9ixLrb
| K7a255GEaUbP/NVFpB/sn56uJc1ijz8u0hWQ3DwVe5ZrHUkNzAuvC2OeQgh2HanV
| 5LwB1nmRZn90PG1puKxktMjXsGY7f9Yvx1/yVnZqu2Ev2uDA0RXij/T+hEqgDMI7
| y4ZFmuYD8z4b2kAUwj7RHh9LUKXKQlO+Pn8hchdR/4IK+Xc4+GFOin0XdQdUJaBD
| 8quOUma424ejF5aB6QCSE82MmHlLBO2tzC9yKv8L8w+fUeQFECH1WfPC56GcAq3U
| IvgdjGrU/7EKN5XkONcCAwEAAaMNMAswCQYDVR0TBAIwADANBgkqhkiG9w0BAQsF
| AAOCAQEAnrOty7WAOpDGhuu0V8FqPoKNwFrqGuQCTeqhQ9LP0bFNhuH34pZ0JFsH
| L+Y/q4Um7+66mNJUFpMDykm51xLY2Y4oDNCzugy+fm5Q0EWKRwrq+hIM+5hs0RdC
| nARP+719ddmUiXF7r7IVP2gK+xqpa8+YcYnLuoXEtpKkrrQCCUiqabltU5yRMR77
| 3wqB54txrB4IhwnXqpO23kTuRNrkG+JqDUkaVpvct+FAdT3PODMONP/oHII3SH9i
| ar/rI9k+4hjlg4NqOoduxX9M+iLJ0Zgj6HAg3EQVn4NHsgmuTgmknbhqTU3o4IwB
| XFnxdxVy0ImGYtvmnZDQCGivDok6jA==
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
31691/tcp open  echo        syn-ack
31790/tcp open  ssl/unknown syn-ack
| ssl-cert: Subject: commonName=bandit
| Issuer: commonName=bandit
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2017-12-28T13:23:40
| Not valid after:  2027-12-26T13:23:40
| MD5:   9ceb 80ed 781c 17a4 55d0 5e61 b001 0ebe
| SHA-1: ab78 886c 4527 c26e 6a4c 7808 aa2d 93d1 5512 e33f
| -----BEGIN CERTIFICATE-----
| MIICsjCCAZqgAwIBAgIJAKZI1xYeoXFuMA0GCSqGSIb3DQEBCwUAMBExDzANBgNV
| BAMMBmJhbmRpdDAeFw0xNzEyMjgxMzIzNDBaFw0yNzEyMjYxMzIzNDBaMBExDzAN
| BgNVBAMMBmJhbmRpdDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAOcX
| ruVcnQUBeHJeNpSYayQExCJmcHzSCktnOnF/H4efWzxvLRWt5z4gYaKvTC9ixLrb
| K7a255GEaUbP/NVFpB/sn56uJc1ijz8u0hWQ3DwVe5ZrHUkNzAuvC2OeQgh2HanV
| 5LwB1nmRZn90PG1puKxktMjXsGY7f9Yvx1/yVnZqu2Ev2uDA0RXij/T+hEqgDMI7
| y4ZFmuYD8z4b2kAUwj7RHh9LUKXKQlO+Pn8hchdR/4IK+Xc4+GFOin0XdQdUJaBD
| 8quOUma424ejF5aB6QCSE82MmHlLBO2tzC9yKv8L8w+fUeQFECH1WfPC56GcAq3U
| IvgdjGrU/7EKN5XkONcCAwEAAaMNMAswCQYDVR0TBAIwADANBgkqhkiG9w0BAQsF
| AAOCAQEAnrOty7WAOpDGhuu0V8FqPoKNwFrqGuQCTeqhQ9LP0bFNhuH34pZ0JFsH
| L+Y/q4Um7+66mNJUFpMDykm51xLY2Y4oDNCzugy+fm5Q0EWKRwrq+hIM+5hs0RdC
| nARP+719ddmUiXF7r7IVP2gK+xqpa8+YcYnLuoXEtpKkrrQCCUiqabltU5yRMR77
| 3wqB54txrB4IhwnXqpO23kTuRNrkG+JqDUkaVpvct+FAdT3PODMONP/oHII3SH9i
| ar/rI9k+4hjlg4NqOoduxX9M+iLJ0Zgj6HAg3EQVn4NHsgmuTgmknbhqTU3o4IwB
| XFnxdxVy0ImGYtvmnZDQCGivDok6jA==
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
31960/tcp open  echo        syn-ack
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port31790-TCP:V=7.01%T=SSL%I=7%D=5/13%Time=5AF827B0%P=x86_64-pc-linux-g
SF:nu%r(GenericLines,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20cu
SF:rrent\x20password\n")%r(GetRequest,31,"Wrong!\x20Please\x20enter\x20the
SF:\x20correct\x20current\x20password\n")%r(HTTPOptions,31,"Wrong!\x20Plea
SF:se\x20enter\x20the\x20correct\x20current\x20password\n")%r(RTSPRequest,
SF:31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x20password\
SF:n")%r(Help,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x
SF:20password\n")%r(SSLSessionReq,31,"Wrong!\x20Please\x20enter\x20the\x20
SF:correct\x20current\x20password\n")%r(TLSSessionReq,31,"Wrong!\x20Please
SF:\x20enter\x20the\x20correct\x20current\x20password\n")%r(Kerberos,31,"W
SF:rong!\x20Please\x20enter\x20the\x20correct\x20current\x20password\n")%r
SF:(FourOhFourRequest,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20c
SF:urrent\x20password\n")%r(LPDString,31,"Wrong!\x20Please\x20enter\x20the
SF:\x20correct\x20current\x20password\n")%r(SIPOptions,31,"Wrong!\x20Pleas
SF:e\x20enter\x20the\x20correct\x20current\x20password\n");
```

* We can see that 5 ports are open between these port ranges.
* Also only two of those ports are running a ssl service.
```
31790/tcp open  ssl/unknown syn-ack
31518/tcp open  ssl/echo    syn-ack
```

* I tried to connect to the first one  in that list.
```
bandit16@bandit:~$ openssl s_client -connect localhost:31790 -ign_eof
```

* Output

```
CONNECTED(00000003)
depth=0 CN = bandit
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = bandit
verify return:1
--
Certificate chain
 0 s:/CN=bandit
   i:/CN=bandit
--
Server certificate
-----BEGIN CERTIFICATE-----
MIICsjCCAZqgAwIBAgIJAKZI1xYeoXFuMA0GCSqGSIb3DQEBCwUAMBExDzANBgNV
BAMMBmJhbmRpdDAeFw0xNzEyMjgxMzIzNDBaFw0yNzEyMjYxMzIzNDBaMBExDzAN
BgNVBAMMBmJhbmRpdDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAOcX
ruVcnQUBeHJeNpSYayQExCJmcHzSCktnOnF/H4efWzxvLRWt5z4gYaKvTC9ixLrb
K7a255GEaUbP/NVFpB/sn56uJc1ijz8u0hWQ3DwVe5ZrHUkNzAuvC2OeQgh2HanV
5LwB1nmRZn90PG1puKxktMjXsGY7f9Yvx1/yVnZqu2Ev2uDA0RXij/T+hEqgDMI7
y4ZFmuYD8z4b2kAUwj7RHh9LUKXKQlO+Pn8hchdR/4IK+Xc4+GFOin0XdQdUJaBD
8quOUma424ejF5aB6QCSE82MmHlLBO2tzC9yKv8L8w+fUeQFECH1WfPC56GcAq3U
IvgdjGrU/7EKN5XkONcCAwEAAaMNMAswCQYDVR0TBAIwADANBgkqhkiG9w0BAQsF
AAOCAQEAnrOty7WAOpDGhuu0V8FqPoKNwFrqGuQCTeqhQ9LP0bFNhuH34pZ0JFsH
L+Y/q4Um7+66mNJUFpMDykm51xLY2Y4oDNCzugy+fm5Q0EWKRwrq+hIM+5hs0RdC
nARP+719ddmUiXF7r7IVP2gK+xqpa8+YcYnLuoXEtpKkrrQCCUiqabltU5yRMR77
3wqB54txrB4IhwnXqpO23kTuRNrkG+JqDUkaVpvct+FAdT3PODMONP/oHII3SH9i
ar/rI9k+4hjlg4NqOoduxX9M+iLJ0Zgj6HAg3EQVn4NHsgmuTgmknbhqTU3o4IwB
XFnxdxVy0ImGYtvmnZDQCGivDok6jA==
-----END CERTIFICATE-----
subject=/CN=bandit
issuer=/CN=bandit
--
No client certificate CA names sent
--
SSL handshake has read 1015 bytes and written 631 bytes
--
New, TLSv1/SSLv3, Cipher is AES128-SHA
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1
    Cipher    : AES128-SHA
    Session-ID: C1BAD8BBD37CD5340B8CB1CD67943DB8CAF9FF47F9E722F1924FC471BEEA6F26
    Session-ID-ctx: 
    Master-Key: B81AE9BAAD4A0B4FFAC7362621A4C78E4330B0D17F5D7F942EB9278B4735CEC627AA9FDEEDDA6C571CA46A385204E38C
    Key-Arg   : None
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - 9a b6 c1 e5 3e 9c 51 03-45 ba c9 c1 cc d7 23 80   ....>.Q.E.....#.
    0010 - ac 25 69 b9 fd 29 a0 40-f6 5f be 9b b6 82 d5 ed   .%i..).@._......
    0020 - f3 7e 54 72 69 a4 17 22-12 ad cd ff c3 d7 34 7b   .~Tri.."......4{
    0030 - 8c 13 c7 2e 38 3d 87 5b-96 7e 86 66 71 ac 39 ff   ....8=.[.~.fq.9.
    0040 - 37 62 42 6e b8 58 b8 66-a7 0f 46 3d 11 51 c1 be   7bBn.X.f..F=.Q..
    0050 - f4 84 ae 44 1e 3c e1 1e-58 8d 7c 1e 5d fa 92 39   ...D.<..X.|.]..9
    0060 - 1c e0 8c 09 a4 78 94 8f-42 79 42 db 5b 30 5d 9f   .....x..ByB.[0].
    0070 - 33 aa e1 b4 29 f3 3d d6-a5 93 35 ca c2 b8 17 26   3...).=...5....&
    0080 - 84 62 46 74 bd 33 47 cc-b4 a6 df 22 bc 73 a2 61   .bFt.3G....".s.a
    0090 - 54 f2 33 66 b9 c5 d6 1d-78 20 62 5e 2f 51 50 8c   T.3f....x b^/QP.

    Start Time: 1526212830
    Timeout   : 300 (sec)
    Verify return code: 18 (self signed certificate)
--
cluFn7wTiGryunymYOu4RcffSxQluehd
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

closed
```
* When i pasted the password for the current level. The server echoed back a private rsa key.

* Now we need to login to the next level bandit17.
* To do this we grab the ssh private key that ssl server echo back. Place that into a text file.
* Then we login.

```
bandit16@bandit:~$ man nmap 
bandit16@bandit:~$ man ssh
bandit16@bandit:~$ mkdir -p /tmp/bandit16/
bandit16@bandit:~$ cd /tmp/bandit16
bandit16@bandit:/tmp/bandit16$ touch sshpass.txt
bandit16@bandit:/tmp/bandit16$ echo "-----BEGIN RSA PRIVATE KEY-----
> MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
> imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
> Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
> DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
> JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
> x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
> KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
> J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
> d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
> YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
> vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
> +TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
> 8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
> SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
> HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
> SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
> R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
> Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
> R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
> L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
> blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
> YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
> 77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
> dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
> vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
> -----END RSA PRIVATE KEY-----
> " > sshpass.txt
```

* Then we need to set the right permission on the file.

```
bandit16@bandit:/tmp/bandit16$ chmod 600 sshpass.txt 
```

* Now we login into the next level bandit17.
```
bandit16@bandit:/tmp/bandit16$ ssh -i sshpass.txt bandit17@localhost
```


---

### Bandit17 -> Level18

* Task

There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

* I ran diff on both files to see the odd line that has been changed.

```
bandit17@bandit:~$ diff passwords.old passwords.new
```
* This gave me the password for the next level.

```
> kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
```

