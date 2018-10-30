### Crypto 1 - 2011

I have decided to improve my skills in ctf competitions, and one way of doing that is by solving pas ctf challenges. That is where i found 
https://365.csaw.io/challenges with resources https://ctf101.org/ to help for background knowledge. I started my quest by first going through
cryptography.

* The challenge asks us to solve;

```
87 101 108 99 111 109 101 32 116 111 32 116 104 101 32 50 48 49 49 32 78 89 85 32 
80 111 108 121 32 67 83 65 87 32 67 84 70 32 101 118 101 110 116 46 32 87 101 32 
104 97 118 101 32 112 108 97 110 110 101 100 32 109 97 110 121 32 99 104 97 108 108 
101 110 103 101 115 32 102 111 114 32 121 111 117 32 97 110 100 32 119 101 32 104 111 
112 101 32 121 111 117 32 104 97 118 101 32 102 117 110 32 115 111 108 118 105 110 103 
32 116 104 101 109 32 97 108 108 46 32 84 104 101 32 107 101 121 32 102 111 114 32 116 
104 105 115 32 99 104 97 108 108 101 110 103 101 32 105 115 32 99 114 121 112 116 111 103 114 97 112 104 121 46
```

* Right away i recognized that this is ascii encoding type all that i needed to was convert these ascii codes in base10 to readable ascii 
characters.

* So i made a quick python program to do this.

```
#!/bin/python3

      
#Loop through the ascii values and convert to characters.
asciiNumbers = [87 ,101 ,108 ,99 ,111 ,109 ,101 ,32 ,116 ,111 ,32 ,116 ,104 ,101 ,32 ,50 ,
48 ,49 ,49 ,32 ,78 ,89 ,85 ,32 ,80 ,111 ,108 ,121 ,32 ,67 ,83 ,65 ,87 ,32 ,67 ,84 ,70 ,32 ,101 ,
118 ,101 ,110 ,116 ,46 ,32 ,87 ,101 ,32 ,104 ,97 ,118 ,101 ,32 ,112 ,108 ,97 ,110 ,110 ,101 ,100 ,
32 ,109 ,97 ,110 ,121 ,32 ,99 ,104 ,97 ,108 ,108 ,101 ,110 ,103 ,101 ,115 ,32 ,102 ,111 ,114 ,32 ,
121 ,111 ,117 ,32 ,97 ,110 ,100 ,32 ,119 ,101 ,32 ,104 ,111 ,112 ,101 ,32 ,121 ,111 ,117 ,32 ,104 ,
97 ,118 ,101 ,32 ,102 ,117 ,110 ,32 ,115 ,111 ,108 ,118 ,105 ,110 ,103 ,32 ,116 ,104 ,101 ,109 ,32 ,
97 ,108 ,108 ,46 ,32 ,84 ,104 ,101 ,32 ,107 ,101 ,121 ,32 ,102 ,111 ,114 ,32 ,116 ,104 ,105 ,115 ,32 ,
99 ,104 ,97 ,108 ,108 ,101 ,110 ,103 ,101 ,32 ,105 ,115 ,32 ,99 ,114 ,121 ,112 ,116 ,111 ,103 ,114 ,97 ,112 ,104 ,121 ,46]


for i in asciiNumbers:
    print(chr(i), end="")

```

* The flag ;

```
Welcome to the 2011 NYU Poly CSAW CTF event. We have planned many challenges for you and we hope you have fun solving them all. 
The key for this challenge is cryptography.
```


### Crypto 2 - 2011

* This task just gives a the following;

```
54:68:69:73:20:69:73:20:74:68:65:20:66:69:
72:73:74:20:6d:65:73:73:61:67:65:20:62:65:
69:6e:67:20:73:65:6e:74:20:74:6f:20:79:6f:
75:20:62:79:20:74:68:65:20:6c:65:61:64:65:
72:73:68:69:70:20:6f:66:20:74:68:65:20:55:
6e:64:65:72:67:72:6f:75:6e:64:20:55:70:72:
69:73:69:6e:67:2e:20:49:66:20:79:6f:75:20:
68:61:76:65:20:64:65:63:6f:64:65:64:20:74:
68:69:73:20:6d:65:73:73:61:67:65:20:63:6f:
72:72:65:63:74:6c:79:20:79:6f:75:20:77:69:
6c:6c:20:6e:6f:77:20:6b:6e:6f:77:20:6f:75:
72:20:6e:65:78:74:20:6d:65:65:74:69:6e:67:
20:77:69:6c:6c:20:62:65:20:68:65:6c:64:20:
6f:6e:20:57:65:64:6e:65:73:64:61:79:20:40:
20:37:70:6d:2e:20:57:65:20:77:69:6c:6c:20:
61:6c:73:6f:20:72:65:71:75:69:72:65:20:61:
20:6b:65:79:20:74:6f:20:62:65:20:6c:65:74:
20:69:6e:74:6f:20:74:68:65:20:6d:65:65:74:
69:6e:67:73:3b:20:74:68:69:73:20:77:65:65:
6b:1f:73:20:6b:65:79:20:77:69:6c:6c:20:62:65:20:6f:76:65:72:74:68:72:6f:77:2e
```

* I recognize that the format is hex encoding.
* so I create a python script to convert hex to ascii readable characters.

```
#!/bin/python3

str = "54:68:69:73:20:69:73:20:74:68:65:20:66:69:72:73:74:20:6d:65:73:73:61:67:65:20:62:65:69:6e:67:20:73:65:6e:74:20:74:6f:20:79:6f:75:20:62:79:20:74:68:65:20:6c:65:61:64:65:72:73:68:69:70:20:6f:66:20:74:68:65:20:55:6e:64:65:72:67:72:6f:75:6e:64:20:55:70:72:69:73:69:6e:67:2e:20:49:66:20:79:6f:75:20:68:61:76:65:20:64:65:63:6f:64:65:64:20:74:68:69:73:20:6d:65:73:73:61:67:65:20:63:6f:72:72:65:63:74:6c:79:20:79:6f:75:20:77:69:6c:6c:20:6e:6f:77:20:6b:6e:6f:77:20:6f:75:72:20:6e:65:78:74:20:6d:65:65:74:69:6e:67:20:77:69:6c:6c:20:62:65:20:68:65:6c:64:20:6f:6e:20:57:65:64:6e:65:73:64:61:79:20:40:20:37:70:6d:2e:20:57:65:20:77:69:6c:6c:20:61:6c:73:6f:20:72:65:71:75:69:72:65:20:61:20:6b:65:79:20:74:6f:20:62:65:20:6c:65:74:20:69:6e:74:6f:20:74:68:65:20:6d:65:65:74:69:6e:67:73:3b:20:74:68:69:73:20:77:65:65:6b:1f:73:20:6b:65:79:20:77:69:6c:6c:20:62:65:20:6f:76:65:72:74:68:72:6f:77:2e"

#strip out the :
str2 = str.replace(":", "")
#Now we convert the encoding to ascii readable format.
print(bytes.fromhex(str2).decode('utf-8'))

```

* So i just strip out the : inbetween each hex digit. 
* Then convert to hex to ascii(utf-8) format.

* I then recieve the flag.

```
This is the first message being sent to you by the leadership of the Underground Uprising. 
If you have decoded this message correctly you will now know our next meeting will be held on Wednesday @ 7pm. 
We will also require a key to be let into the meetings; this weeks key will be overthrow.
```


### Crypto 3 - 2011

* This challenges give a string of bits.
* I first place this in a file as its pretty long.

```
#!/bin/python3 
import binascii

#Convert binary to ascii.

#Open file containing binary.
FILE = open("binarydata", "r+")

#Read the file.
binaryFile = FILE.read()

#Convert binary to ascii.
n = int(binaryFile, 2)

#Display the ascii representation.
print(binascii.unhexlify('%x' % n))
```

* I use binascii to convert from binary to ascii.
* The solution;

```
Last weeks meeting was a great success. We seem to be generating a lot of buzz about the movement. 
The key for next weeks meeting is resistance. 
If there is anyone else you know of that may be interested in joining bring them to the meeting this week. It will be held same time, same place.'
```

### Crypto 4 - 2011

* This challenge was pretty straightfoward there was base64 encoded
string and all that was needed to decoding this string.

* I used the base64 module from python to complete this challenge.

```
#!/bin/python3
import base64

#Base64 decode to ascii.
string = "VGhhdCBtZWV0aW5nIHdhcyBhIGxpdHRsZSBjcmF6eS4gV2UgaGF2ZSBubyBpZGVhIHdoZXJlIHRob3NlIGd1eXMgaW4gdGhlIGJsYWNrIHN1aXRzIGNhbWUgZnJvbSwgYnV0IHdlIGFyZSBsb29raW5nIGludG8gaXQuIFVzZSB0aGUga2V5IGluZmlsdHJhdGlvbiBmb3IgbmV4dCB3ZWVrknMgbWVldGluZy4gU3RheSB3aXRoIHRoZSBjYXVzZSBhbmQgd2Ugd2lsbCBzdWNjZWVkLg=="

print(base64.b64decode(string))
```

* Flag is;

```
That meeting was a little crazy. We have no idea where those guys in the black suits came from, but we are looking into it. Use the key infiltration for next week\x92s meeting. Stay with the cause and we will succeed.
```

