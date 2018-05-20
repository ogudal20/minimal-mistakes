I'm going to try and further my knowledge in bash and bash scripting. 
This will be done through hackerrank challenges on bash track.The link to challenges is below.
[Hackerrank Bash Challenges](https://www.hackerrank.com/domains/shell/bash)

### Challenge 1 - Let's Echo

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


### Challenge 2 - Looping and Skipping

 
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


---

### Challenge 3 - A personalized Echo

Write a Bash script which accepts as input and displays a greeting: "Welcome (name)"


* Algorithm

1. Read in name from stdin. Store in a variable.
2. Output Welcome followed by name variable.

* Code

```
#!/bin/bash

#Bash takes a name as input then displays Welcome [name]

#Read in a name from the user.
read -p "Enter in a name: " name

#Display Welcome name.
echo Welcome $name
```


---

### Challenge 4 - Looping with Numbers

Use for loops to display the natural numbers from 1 to 50.

* Algorithm

1. Create a loop that iterates from 1 to 50
2. Display each number in the loop.


* Code

```
#!/bin/bash

#Bash script to loop through 1 to 50 and display them.
for i in {1..50};
do
    echo $i
done
```


