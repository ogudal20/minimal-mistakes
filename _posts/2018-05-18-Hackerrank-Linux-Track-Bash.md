I'm going to try and further my knowledge in bash and bash scripting. 
This will be done through hackerrank challenges on bash track.The link to challenges is below.
[Hackerrank Bash Challenges](https://www.hackerrank.com/domains/shell/bash)

* Challenge 1 - Let's Echo

Write a bash script which does just one thing: saying "HELLO".

Input Format

There is no input file required for this problem.

Output Format

HELLO

Sample Input

-

Sample Output

HELLO

* Solution


```bash
#!/bin/bash

echo "HELLO"
```

---


* Challenge 2 - Looping and Skipping

 
Your task is to use for loops to display only odd natural numbers from to .


* Algorithm

1. Loop from 1 to 99.
2. Check if the number is odd.
3. Display the number is it is odd number
4. Else does not display number.

* Code

```
#!/bin/bash

#Script to display odd numbers from 1 to 99.
for i in {1..99};
do
    #Check if i is a odd number.
    if(($i % 2 != 0))
    then
        echo $i
    fi
done
```


