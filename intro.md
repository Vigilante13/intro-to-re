## Introduction
Reverse Engineering, in simple terms, can be described as a way to deconstruct code to extract its designs, architechture, or certain knowledge. Both hardware and software engineers use reverse engineering when debugging code or analyzing a foreign program they encountered with no source code available. The basic techinques of reverse engineering can be applied to any situation involving a simple piece of code.

When you write a program, you often don't think about how you wrote the program. You're given a task to code a specific functionality and you write that program in the style that you were taught or learnt. Reverse engineering makes you view your code in a more abstract manner. 

- If someone wrote a similar program, could you identify it by simply looking at the assembly or machine code? 
- If someone has a bug in their program, could you easily identify the issue? 
- Are there any system functions you could replace in your code that are more secure and don't have any vulnerabilities to avoid potential hacks? 

These are just a few examples that we ask ourselves about the code we write. Having a deeper understanding of code, beyond the surface-level functionality, is a critical technique in reverse engineering. 

Let's dive into a simple `C` program that I wrote to illustrate the magic of Reverse Engineering:
```c
#include <unistd.h>
#include <stdio.h>
#include <stdint.h>

int secret_function(int input) {
    return (((input >> 2) - 1) ^ 48);
}

int main() {
    int x;
    printf("Please enter a number to check:\n");
    printf("> ");
    scanf("%d", &x);
    if (secret_function(x) == 91) {
        printf("Congrats! You got it!\n");
    } else {
        printf("Nope. Sorry...\n");
    }
}
```

So, our program is very simple. It prompts the user for an int and passes it through a function that checks if the result is `91`.

Normally, a person would just brute force integer values starting from 0 until they get the desired result, since `secret_function` is just computing an integer value. 

However, this is where I want to demonstrate why understanding the program is essential.

If we take a look at `secret_function`, we see that it takes our input and does the following:

```c 
(((input >> 2) - 1) ^ 48)
```

Notice that the computations done here can easily be reversed!

`>> 2` represents a right shift by the amount of ```2```. To reverse it, we can simply do `<< 2`, which is a left shift of amount `2.`

Similarly, `- 1` reversed is `+ 1`. Then finally we have `^ 48.` The properties of xor tells us that:
```c
a ^ b = c
b ^ c = a
```

So, we can reverse this by doing `48 ^ 91`, where `91` is our desired output.

Combining all of this together, in order to reverse:

```c 
(((input >> 2) - 1) ^ 48) == 91
```

We can do the following:

```c 
((91 ^ 48) + 1) << 2
```

Calculating this gives us `432`. Lets see if it works:
```shell
varuniyer@VarunPC:/mnt/c/Users/vi021/Desktop$ ./example
Please enter a number to check:
> 432
Congrats! You got it!
```
    
Analyzing our source code can help us avoid using brute force to solve program checkers. In the next section I'm going to over Tools that reverse engineers use to understand program flow and construct scripts to crack the program.
