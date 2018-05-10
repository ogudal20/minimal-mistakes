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

