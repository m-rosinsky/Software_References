# Strings in C

Strings in C have an extra layer of difficult (no surprises there) because there isn't a native string data type in the language.

Instead, when using strings in C, we have to work with an array of ```char```.

This is why the pointer references and array references come before this one in the repo.

---

### Declaring Strings

Strings, being that they're simply an array of ```char``` can be declared in the exact same way arrays can.

For example, we can write:

```C
char my_string[5];
```

to create a string of size 5.

The C compiler is also capable of figuring out the size of a given string for us, using this notation:

```C
char * my_string = "hello, world!";
```

In this case, ```my_string``` has the length of ```13```, even though we didn't explicitly say it.

---

### The Null Terminator

In C, all strings are terminated by what's called a _null terminator_. Every character in C corresponds to some ASCII value, defined in an ASCII table.

Each of these character values take up 1 byte in memory. The last character in a string always has an ASCII value of 0, or NULL.

This is so the compiler knows where the string ends when going through it in memory.

For example, in the piece of code:

```C
char * my_string = "hello";
```

```my_string``` would look like this in memory:

| 0x00 | 0x01 | 0x02 | 0x03 | 0x04 | 0x05 | 0x06 |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| my_string | 'h' | 'e' | 'l' | 'l' | 'o' | '\0' |

Remember from how arrays work in C, that ```my_string``` is a pointer to the first character of the actual array. In this case, 0x01.

At the end, we see the null terminator right after the last character in the array.

---

### String Printing

We can use the ```%s``` format specifier to print strings, like so:

```C
#include <stdio.h>

int main()
{
    char * my_string = "Hello, World!";
    
    printf("%s\n", my_string);
}
```
```
Hello, World!
```

The ```%s``` format specifier will tell the compiler to print through the string character by character, until it reaches the null terminator.

---

### The C String Library

C contains a number of library functions for performing operations on strings. This tutorial will not be exhaustive, being that the string library is fairly large, but we can cover a few key ones here.

Most of the string library functions are contained in the header ```string.h```, so we can include that.

```C
#include <stdio.h>
#include <string.h>

int main()
{

}
```

Some of the functions this library contains are (not limited to):

- ```strlen``` - get the length of a string
- ```strcmp``` - compare two strings together
- ```strcpy``` - copy one string's contents into another.
- ```memcpy``` - copy memory contents from one place to another
- ```memset``` - set bytes in a location in memory to a given value

---

### Strlen

The ```strlen``` function in C returns the length of a string. This goes through the string, counting the size, until it reaches the null terminator.

Note that the size it returns does NOT include the null terminator.

```C
#include <stdio.h>
#include <string.h>

int main()
{
    char * my_string = "hello";
    
    // strlen returns a size_t data type.
    size_t my_string_len = strlen(my_string);
    
    printf("%s is %i characters long\n", my_string, my_string_len);
}
```
```
hello is 5 characters long
```

---

### Strcmp

```strcmp``` compares two strings. It returns 0 if the strings are equal, or nonzero if they are not equal.

```C
#include <stdio.h>
#include <string.h>

int main()
{
    char * string_1 = "hello";
    char * string_2 = "hallo";
    
    if (0 == strcmp(string_1, string_2))
    {
        printf("Strings are equal!\n");
    }
    else
    {
        printf("Strings are NOT equal!\n");
    }
}
```
```
Strings are NOT equal!
```

---

### Strcpy

```strcpy``` copies the contents of one string (including the null terminator) into another string.

_NOTE_: If the destination string of ```strcpy``` is not large enough, a buffer overflow occurs. This is a large security vulnerability, so this function should be avoided all together in favor of ```strncpy```, which lets you specify a maximum number of bytes to copy, or ```memcpy```.

```C
#include <stdio.h>
#include <string.h>

int main()
{
     char string_1 = "word";
     char string_2 = "bigger word";
     
     strcpy(string_1, string_2); // BAD: BUFFER OVERFLOW. string_1 is not big enough.
}
```

---

### Memcpy

The ```memcpy``` function copies a specified number of bytes from one location in memory to another. It's parameters are:

```C
memcpy(dst_addr, src_addr, num_bytes);
```

This function has applications outside of just strings, being that it can copy any kind of memory.

```C
#include <stdio.h>
#include <string.h>

int main()
{
     char * string_1 = "Hello, World!";
     
     char string_2[5]; // Create a new string of fixed length (5).
     
     // Copy the first 5 bytes of string_1 to string_2
     memcpy(string_2, string_1, 5);
     
     printf("%s\n", string_2);
}
```
```
Hello
```

This also has applications to things other than strings, for example integer arrays.
```C
#include <stdio.h>
#include <string.h>

int main()
{
    int arr_1[5] = {1, 2, 3, 4, 5};
    int arr_2[3];
    
    /*
    Copy the first 3 elements of arr_1 to arr_2.
    
    We use sizeof(int) to get the number of bytes in
    a single int, and then multiply it by 3
    */
    memcpy(arr_2, arr_1, 3 * sizeof(int));
}
```

---

### Memset

```memset``` sets a given block of memory to a specified value. The parameters are:

```C
memset(addr, value, num_bytes);
```

One instance this can be used in is "zeroing out" an array. For example:

```C
#include <stdio.h>
#include <string.h>

int main()
{
    int arr[3] = {1,2,3};
    
    // Set all elements of arr to 0 with memset.
    memset(arr, 0, 3 * sizeof(int));
    
    // Print the array.
    for (size_t i = 0; i < 3; i++)
    {
       printf("%d ", arr[i]);
    }
}
```
```
0 0 0
```

---

### Exercises

Check out the exercises within this folder for some extra practice!
