# Tools
In this section I'm going to cover various tools that software/hardware engineers use when attempting to reverse engineering a piece of code.

## Decompiler 
A decompiler is a tool that attempts to translate machine code into pseudocode by analyzing patterns in the machine code and attempting to correlate it to previously decompiled blocks of pseudocode.

IDA offers HexRays, which translates machine code into a higher language pseudocode.

## IDA Decompilation Example

Lets compare our `C` code in our example program to what IDA's attempt at decompiling it.

Example `C` Code:
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

IDA Decompiled `C` Code:
```c
//----- (0000000000001155) ----------------------------------------------------
__int64 __fastcall secret_function(signed int a1)
{
  return ((a1 >> 2) - 1) ^ 0x30u;
}

//----- (000000000000116A) ----------------------------------------------------
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int v4; // [sp+Ch] [bp-4h]@1

  puts("Please enter a number to check:");
  printf("> ", argv);
  __isoc99_scanf("%d", &v4);
  if ( (unsigned int)secret_function(v4) == 91 )
    puts("Congrats! You got it!");
  else
    puts("Nope. Sorry...");
  return 0;
}
// 1050: using guessed type int __fastcall __isoc99_scanf(_QWORD, _QWORD);
```

IDA uses the system function name, but besides that it appears IDA has correctly decompiled our `C` program! The robustness of a simple decompiler can assist in various techniques of reverse engineering a program.

This is all the tools I have to discuss. In the final section I will talk about [Jobs/Interviews](https://vigilante13.github.io/intro-to-re/jobs.html)
