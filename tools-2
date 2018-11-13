# Tools
In this section I'm going to cover various tools that software/hardware engineers use when attempting to reverse engineering a piece of code.

## Debugger 
A diassembler is a tool that allows you to execute a program step by step. You can insert breakpoints and print out register values to trace program execution. These utilities are very useful to understand what a program is doing.

### Debugging with GDB
Using our example program from earlier, here is how we can debug it using GDB, and see what happens to our input at the end `secret_function`.

First we set a breakpoint for `secret_function` and wait for gdb to catch it:
```gdb
(gdb) disas 0x8001155
Dump of assembler code for function secret_function:
   0x0000000008001155 <+0>:     push   %rbp
   0x0000000008001156 <+1>:     mov    %rsp,%rbp
   0x0000000008001159 <+4>:     mov    %edi,-0x4(%rbp)
   0x000000000800115c <+7>:     mov    -0x4(%rbp),%eax
   0x000000000800115f <+10>:    sar    $0x2,%eax
   0x0000000008001162 <+13>:    sub    $0x1,%eax
   0x0000000008001165 <+16>:    xor    $0x30,%eax
   0x0000000008001168 <+19>:    pop    %rbp
   0x0000000008001169 <+20>:    retq
End of assembler dump.
(gdb) b *0x8001155
Breakpoint 4 at 0x8001155
(gdb) c
Continuing.
Please enter a number to check:
> 10
Breakpoint 4, 0x0000000008001155 in secret_function ()
```
Perfect. Now we step until we reach the address `0x0000000008001168`, since that will be after the computations to our input.
```gdb
(gdb) si
0x0000000008001156 in secret_function ()
(gdb) si
0x0000000008001159 in secret_function ()
(gdb) si
0x000000000800115c in secret_function ()
(gdb) si
0x000000000800115f in secret_function ()
(gdb) si
0x0000000008001162 in secret_function ()
(gdb) si
0x0000000008001165 in secret_function ()
(gdb) si
0x0000000008001168 in secret_function ()
(gdb) i r
rax            0x31     49
rbx            0x0      0
rcx            0x0      0
rdx            0x7fffff7a98d0   140737479612624
rsi            0x1      1
rdi            0xa      10
rbp            0x7ffffffedd30   0x7ffffffedd30
rsp            0x7ffffffedd30   0x7ffffffedd30
r8             0x7ffffffed802   140737488279554
r9             0xa      10
r10            0x0      0
r11            0xa      10
r12            0x8001070        134221936
r13            0x7ffffffede30   140737488281136
r14            0x0      0
r15            0x0      0
rip            0x8001168        0x8001168 <secret_function+19>
eflags         0x202    [ IF ]
cs             0x33     51
---Type <return> to continue, or q <return> to quit---
ss             0x2b     43
ds             0x0      0
es             0x0      0
fs             0x0      0
gs             0x0      0
(gdb)
```

So our result was stored in `eax`, from the assembly code. Looking at the registers, we can see that `eax = 49`

If we manually calcuate from our input of `10`:

```c
(((10 >> 2) - 1) ^ 48) = 49
```

So, this is an example of how we can use a debugger to step through the execution of a program and note any relevant features of it.
