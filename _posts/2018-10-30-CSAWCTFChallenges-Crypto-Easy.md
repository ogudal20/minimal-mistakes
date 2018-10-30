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


