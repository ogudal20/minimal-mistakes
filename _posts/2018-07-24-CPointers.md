### What is a Pointer

* A pointer, points to a memory location where data is stored.

---

### Pointer Variables

* In this section i will give a example of a pointer variable.

```
#include <stdio.h>

int main(int argc, char ** argv){

	int num;
	int *numPtr;
	int num2;

	num = 100;
	numPtr = &num;
	num2 = *numPtr;

	printf("num=%d, numPtr=%d, address of num=%d, num2=%d\n", num, numPtr, &num, num2);

	return 0;
}
```

* Output

```
num=100, numPtr=973540928, address of num=973540928, num2=100
```

* First i initialize num integer this holds the value 100.
* Then i initialize a int pointer numPtr.
* I then set the address of num to numPtr.
* I finally de-references the numPtr pointer variable and set that to num2 variable which outputs 100.


