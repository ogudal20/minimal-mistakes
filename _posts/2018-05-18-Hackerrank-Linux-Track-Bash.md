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


---

### Challenge 5 - The World of Numbers

Given two integers, x and y , find their sum, difference, product, and quotient.


* Algorithm 

1. Read in two numbers from stdin.
2. Add them, subtract, product and divide.
3. Display the results.

* Code

```
/bin/bash

#Read in two numbers from the user.
read -p "Enter in the first number: " first_num
read -p "Enter in a second number: " second_num

#Arithmetic
expr $first_num + $second_num
expr $first_num - $second_num
expr $first_num \* $second_num   #Escape * As this will display all files in the current directory.
expr $first_num / $second_num
```


---

### Challenge 6 - Comparing Numbers

Given two integers, x and y , identify whether x < y or x > y or x = y. 

* Algorithm

1. Read in a number.
2. Read in another number
3. Check if the first number is greater than the second number.
4. Check if the first number is less than second number.
5. Else check if they are equal.

* Code

```
!/bin/bash

#Read in two numbers from stdin.
read -p "Enter in a number: " first_num
read -p "Enter in a second number: " second_num

#Check if the first number is greater than the second.
if [ $first_num -gt $second_num ]
then
	echo "X is greater than Y"
	
elif [ $first_num -lt $second_num ]
then
	echo "X is less than Y"

else
	echo "X is equal to Y"

fi

```

---

### Challenge 7 - Getting started with conditionals

Read in one character from the user (this may be 'Y', 'y', 'N', 'n'). If the character is 'Y' or 'y' display "YES". If the character is 'N' or 'n' display "NO". No other character will be provided as input. 


* Algorithm

1. Read in a character.
2. Check if the character is Y or y if it is, then display YES
3. Else display NO
4. end

* Code

```
#!/bin/bash 

#Read in a character

read -p "Enter in a character: " char


#Check if the input if y or Y then display YES
#-o stands for or
#whereas -a stands for and.

if [ $char == "y" -o $char == "Y" ]
then 
    echo "YES"
else
    echo "NO"
fi
```

---

### Challenge 8 - More on Conditionals

Given three integers (X,Y ,and Z) representing the three sides of a triangle, identify whether the triangle is Scalene, Isosceles, or Equilateral.

* Algorithm

1. Read in three integers for the sides of the triangle.
2. Check if the triangle is a equilateral by checking if all sides are equal.
3. Check if the triangle is a isosceles by checking if two sides are equal.
4. Else no sides are equal then its scalene


* Code

```
#!/bin/bash

#Read in three integers for the sides of a triangle.
read -p "Enter in the first side of the triangle: " first_side
read -p "Enter in the second side of the triangle: " second_side
read -p "Enter in the third side of the triangle: " third_side

#Check if the triangle is equaliateral.
if [ $first_side == $second_side -a $third_side == $second_side ]
then
	echo "EQUILATERAL"

#Now check if the triangle is isosceles triangle.
elif [ $first_side == $second_side -o $third_side == $second_side ]
then
	echo "ISOSCELES"

#If these conditions don't meet then we display SCALENE
else
	echo "SCALENE"
fi
```


