## Introduction
Reverse Engineering, in simple terms, can be described as a way to deconstruct code to extract its designs, architechture, or certain knowledge. Both hardware and software engineers use reverse engineering when debugging code or analyzing a foreign program they encountered with no source code available. The basic techinques of reverse engineering can be applied to any situation involving a simple piece of code.

When you write a program, you often don't think about how you wrote the program. You're given a task to code a specific functionality and you write that program in the style that you were taught or learnt. Reverse engineering makes you view your code in a more abstract manner. 

- If someone wrote a similar program, could you identify it by simply looking at the assembly or machine code? 
- If someone has a bug in their program, could you easily identify the issue? 
- Are there any system functions you could replace in your code that are more secure and don't have any vulnerabilities to avoid potential hacks? 

These are just a few examples that we ask ourselves about the code we write. Having a deeper understanding of code, beyond the surface-level functionality, is a critical technique in reverse engineering. 

Let's dive into a simple C program that I wrote to illustrate the magic of Reverse Engineering:

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

So, our program is very simple. It prompts the user for an int and passes it through a function that checks if the result is `91`.
