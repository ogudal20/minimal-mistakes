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

