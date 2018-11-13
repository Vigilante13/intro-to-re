# Tools
In this section I'm going to cover various tools that software/hardware engineers use when attempting to reverse engineering a piece of code.

## Decompiler 
A decompiler is a tool that attempts to translate machine code into pseudocode by analyzing patterns in the machine code and attempting to correlate it to previously decompiled blocks of pseudocode.

IDA offers HexRays, which translates machine code into a higher language pseudocode.

## IDA Decompilation Example

Lets compare our `C` code in our example program to what IDA's attempt at decompiling it.

