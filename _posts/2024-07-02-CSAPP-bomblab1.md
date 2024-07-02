---
title: 'CSAPP Bomb Lab - 1'
date: 2024-07-02
permalink: /posts/2024/07/csapp/bomblab-day1&2
tags:
  - CSAPP
  - bomb lab
  - cool thing
---
Recently I begin to write some posts to record my efforts (and pain) during the study.

# Preparation
1. get the bomb lab resource from [CMU CSAPP official website](https://csapp.cs.cmu.edu/3e/labs.html). I download the `README.md`, `Writeup` and `Self-Study Handout`. The hints in the `Writeup` pdf is helpful, which directs me to browse some useful tools' handbook in this lab, e.g. [2-pages GDB handbook](https://csapp.cs.cmu.edu/2e/docs/gdbnotes-x86-64.pdf).
2. If you are a self-studier and non-CMU student like me, you can skip most of the README instructions about how CMU students should get their unique bomb and submit their project. For others, the only thing you have to do is to **decompress the handout, find the binary exe file, and begin to defuse the bomb.**

# Try
After opening the `bomb.c` file and browsing the code structure, I find that the bomb has 6 phases, which menas 6 sub-challenges to conquer. So let's do it step by step.

## Phase 1
For every phase, the first thing to do is `gdb bomb` to enter gdb mode. Then, `disas phase_{x}` to disassemble the phase_x function and decode it.

Dump of assembler code for function phase_1:
```
   0x0000000000400ee0 <+0>:     sub    $0x8,%rsp
   0x0000000000400ee4 <+4>:     mov    $0x402400,%esi
   0x0000000000400ee9 <+9>:     callq  0x401338 <strings_not_equal>
   0x0000000000400eee <+14>:    test   %eax,%eax
   0x0000000000400ef0 <+16>:    je     0x400ef7 <phase_1+23>
   0x0000000000400ef2 <+18>:    callq  0x40143a <explode_bomb>
   0x0000000000400ef7 <+23>:    add    $0x8,%rsp
   0x0000000000400efb <+27>:    retq   
```
We can see the core part is the `strings_not_equal` function. If the return value of the function is zero, phase 1 will pass. So, lets dive into `strings_not_equal`.

> Q: Why does the stack pointer deduced by 8 at first?  
> A: For memory alignment. Every time before calling procedure, you have to ensure the stack pointer is **16x**. We assume that before entering `phase_1`, the stack pointer is aligned. After calling `phase_1`, the `%rsp` will be deduced by 8 because the returning address is pushed into stack. So, we need to deduct another 8 to align it.

### strings_not_eqaul
Dump of assembler code for function strings_not_equal:
```
   0x0000000000401338 <+0>:     push   %r12
   0x000000000040133a <+2>:     push   %rbp
   0x000000000040133b <+3>:     push   %rbx
   0x000000000040133c <+4>:     mov    %rdi,%rbx
   0x000000000040133f <+7>:     mov    %rsi,%rbp
   0x0000000000401342 <+10>:    callq  0x40131b <string_length>
   0x0000000000401347 <+15>:    mov    %eax,%r12d
   0x000000000040134a <+18>:    mov    %rbp,%rdi
   0x000000000040134d <+21>:    callq  0x40131b <string_length>
   0x0000000000401352 <+26>:    mov    $0x1,%edx
   0x0000000000401357 <+31>:    cmp    %eax,%r12d
   0x000000000040135a <+34>:    jne    0x40139b <strings_not_equal+99>
   0x000000000040135c <+36>:    movzbl (%rbx),%eax
   0x000000000040135f <+39>:    test   %al,%al
   0x0000000000401361 <+41>:    je     0x401388 <strings_not_equal+80>
   0x0000000000401363 <+43>:    cmp    0x0(%rbp),%al
   0x0000000000401366 <+46>:    je     0x401372 <strings_not_equal+58>
   0x0000000000401368 <+48>:    jmp    0x40138f <strings_not_equal+87>
   0x000000000040136a <+50>:    cmp    0x0(%rbp),%al
   0x000000000040136d <+53>:    nopl   (%rax)
   0x0000000000401370 <+56>:    jne    0x401396 <strings_not_equal+94>
   0x0000000000401372 <+58>:    add    $0x1,%rbx
   0x0000000000401376 <+62>:    add    $0x1,%rbp
   0x000000000040137a <+66>:    movzbl (%rbx),%eax
   0x000000000040137d <+69>:    test   %al,%al
   0x000000000040137f <+71>:    jne    0x40136a <strings_not_equal+50>
   0x0000000000401381 <+73>:    mov    $0x0,%edx
   0x0000000000401386 <+78>:    jmp    0x40139b <strings_not_equal+99>
   0x0000000000401388 <+80>:    mov    $0x0,%edx
   0x000000000040138d <+85>:    jmp    0x40139b <strings_not_equal+99>
   0x000000000040138f <+87>:    mov    $0x1,%edx
   0x0000000000401394 <+92>:    jmp    0x40139b <strings_not_equal+99>
   0x0000000000401396 <+94>:    mov    $0x1,%edx
   0x000000000040139b <+99>:    mov    %edx,%eax
   0x000000000040139d <+101>:   pop    %rbx
   0x000000000040139e <+102>:   pop    %rbp
   0x000000000040139f <+103>:   pop    %r12
   0x00000000004013a1 <+105>:   retq   
```
From the name of the function, we can guess it compares two strings. One parameter must be the string in `%rsi`, the other must be the string in `%rdi`.

For the [`string_length`](#string_length) function, we can guess it calculates the length of the string.

If the length of strings are not equal, return 1.

Then come to *line +36*: move the memory content in `%rbx` to `%eax` and zero-extended to 64 bits. Now `%rax` lies the firs 4 characters of the input string. The following code is just to ensure the input is not a `\n`, and the input must be the same as the string located at `0x402400`, which is indicated in the second line of `phase_1`. 

**Answer: "Border relations with Canada have never been better."**


### string_length
Dump of assembler code for function string_length:
```
   0x000000000040131b <+0>:     cmpb   $0x0,(%rdi)
   0x000000000040131e <+3>:     je     0x401332 <string_length+23>
   0x0000000000401320 <+5>:     mov    %rdi,%rdx
   0x0000000000401323 <+8>:     add    $0x1,%rdx
   0x0000000000401327 <+12>:    mov    %edx,%eax
   0x0000000000401329 <+14>:    sub    %edi,%eax
   0x000000000040132b <+16>:    cmpb   $0x0,(%rdx)
   0x000000000040132e <+19>:    jne    0x401323 <string_length+8>
   0x0000000000401330 <+21>:    repz retq 
   0x0000000000401332 <+23>:    mov    $0x0,%eax
   0x0000000000401337 <+28>:    retq   
```
`%rdi` must store the **address of the beginning character of the string**, because the memory accessing method. If this is a null ending in C, procedure ends and return 0. The following code is a loop, counting the number of non-null characters and returning the count.

## Phase 2
Dump of assembler code for function phase_2:
```
   0x0000000000400efc <+0>:	push   %rbp
   0x0000000000400efd <+1>:	push   %rbx
   0x0000000000400efe <+2>:	sub    $0x28,%rsp
   0x0000000000400f02 <+6>:	mov    %rsp,%rsi
   0x0000000000400f05 <+9>:	callq  0x40145c <read_six_numbers>
   0x0000000000400f0a <+14>:	cmpl   $0x1,(%rsp)
   0x0000000000400f0e <+18>:	je     0x400f30 <phase_2+52>
   0x0000000000400f10 <+20>:	callq  0x40143a <explode_bomb>
   0x0000000000400f15 <+25>:	jmp    0x400f30 <phase_2+52>
   0x0000000000400f17 <+27>:	mov    -0x4(%rbx),%eax
   0x0000000000400f1a <+30>:	add    %eax,%eax
   0x0000000000400f1c <+32>:	cmp    %eax,(%rbx)
   0x0000000000400f1e <+34>:	je     0x400f25 <phase_2+41>
   0x0000000000400f20 <+36>:	callq  0x40143a <explode_bomb>
   0x0000000000400f25 <+41>:	add    $0x4,%rbx
   0x0000000000400f29 <+45>:	cmp    %rbp,%rbx
   0x0000000000400f2c <+48>:	jne    0x400f17 <phase_2+27>
   0x0000000000400f2e <+50>:	jmp    0x400f3c <phase_2+64>
   0x0000000000400f30 <+52>:	lea    0x4(%rsp),%rbx
   0x0000000000400f35 <+57>:	lea    0x18(%rsp),%rbp
   0x0000000000400f3a <+62>:	jmp    0x400f17 <phase_2+27>
   0x0000000000400f3c <+64>:	add    $0x28,%rsp
---Type <return> to continue, or q <return> to quit---
   0x0000000000400f40 <+68>:	pop    %rbx
   0x0000000000400f41 <+69>:	pop    %rbp
   0x0000000000400f42 <+70>:	retq   
```
Now we have to be familiar with some registers. ![image from textbook](https://wuzhoudu.github.io/files/CSAPP_labs/bomblab/commonRegisters.png) The `%rdi` register is responsible for passing the argument, so other 5 registers! Here, the `%rsi` is the second argument, so we can dive into the `read_six_numbers` procedure to see how the stack pointer works.

After that, the only thing we need to do is simulation, and I find that the wanted input is **1 2 4 8 16 32**.

### read_six_numbers
Dump of assembler code for function read_six_numbers:
```
   0x000000000040145c <+0>:	sub    $0x18,%rsp
   0x0000000000401460 <+4>:	mov    %rsi,%rdx
   0x0000000000401463 <+7>:	lea    0x4(%rsi),%rcx
   0x0000000000401467 <+11>:	lea    0x14(%rsi),%rax
   0x000000000040146b <+15>:	mov    %rax,0x8(%rsp)
   0x0000000000401470 <+20>:	lea    0x10(%rsi),%rax
   0x0000000000401474 <+24>:	mov    %rax,(%rsp)
   0x0000000000401478 <+28>:	lea    0xc(%rsi),%r9
   0x000000000040147c <+32>:	lea    0x8(%rsi),%r8
   0x0000000000401480 <+36>:	mov    $0x4025c3,%esi
   0x0000000000401485 <+41>:	mov    $0x0,%eax
   0x000000000040148a <+46>:	callq  0x400bf0 <__isoc99_sscanf@plt>
   0x000000000040148f <+51>:	cmp    $0x5,%eax
   0x0000000000401492 <+54>:	jg     0x401499 <read_six_numbers+61>
   0x0000000000401494 <+56>:	callq  0x40143a <explode_bomb>
   0x0000000000401499 <+61>:	add    $0x18,%rsp
   0x000000000040149d <+65>:	retq   
```
The core part of this procedure is to understand what is `sscanf` in C. Here, we should pass 8 parameters to it. However, when the number of arugments is over 6, we have to store the extra parameters into the stack, like line *+15* and *+24*. You can find all the arguments based on the image above, accoridng to ABI (Application Binary Interface), the traditional usage of registers. 

So, `read_six_numbers` procedure just parse 6 decimal numbers from the input string and stores them in the stack. After decoding what this procedure is about, we can go back to `phase_2` procedure. 

## Phase 3
Dump of assembler code for function phase_3:
```
   0x0000000000400f43 <+0>:	sub    $0x18,%rsp
   0x0000000000400f47 <+4>:	lea    0xc(%rsp),%rcx
   0x0000000000400f4c <+9>:	lea    0x8(%rsp),%rdx
   0x0000000000400f51 <+14>:	mov    $0x4025cf,%esi
   0x0000000000400f56 <+19>:	mov    $0x0,%eax
   0x0000000000400f5b <+24>:	callq  0x400bf0 <__isoc99_sscanf@plt>
   0x0000000000400f60 <+29>:	cmp    $0x1,%eax
   0x0000000000400f63 <+32>:	jg     0x400f6a <phase_3+39>
   0x0000000000400f65 <+34>:	callq  0x40143a <explode_bomb>
   0x0000000000400f6a <+39>:	cmpl   $0x7,0x8(%rsp)
   0x0000000000400f6f <+44>:	ja     0x400fad <phase_3+106>
   0x0000000000400f71 <+46>:	mov    0x8(%rsp),%eax
   0x0000000000400f75 <+50>:	jmpq   *0x402470(,%rax,8)
   0x0000000000400f7c <+57>:	mov    $0xcf,%eax
   0x0000000000400f81 <+62>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400f83 <+64>:	mov    $0x2c3,%eax
   0x0000000000400f88 <+69>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400f8a <+71>:	mov    $0x100,%eax
   0x0000000000400f8f <+76>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400f91 <+78>:	mov    $0x185,%eax
   0x0000000000400f96 <+83>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400f98 <+85>:	mov    $0xce,%eax
---Type <return> to continue, or q <return> to quit---
   0x0000000000400f9d <+90>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400f9f <+92>:	mov    $0x2aa,%eax
   0x0000000000400fa4 <+97>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400fa6 <+99>:	mov    $0x147,%eax
   0x0000000000400fab <+104>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400fad <+106>:	callq  0x40143a <explode_bomb>
   0x0000000000400fb2 <+111>:	mov    $0x0,%eax
   0x0000000000400fb7 <+116>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400fb9 <+118>:	mov    $0x137,%eax
   0x0000000000400fbe <+123>:	cmp    0xc(%rsp),%eax
   0x0000000000400fc2 <+127>:	je     0x400fc9 <phase_3+134>
   0x0000000000400fc4 <+129>:	callq  0x40143a <explode_bomb>
   0x0000000000400fc9 <+134>:	add    $0x18,%rsp
   0x0000000000400fcd <+138>:	retq   
```
This phase is interesting. The code before *line +24* is easy to understand, which is parsing two decimal from the input and storing them in the stack. The most interesting part is *line +50*, where the code unconditionally jumps to an address according to the first input number! I was stuck for quite a while to thinking. After some trials like the first number is 0, 1, ..., I find that there are multiple groups of input leading to a valid result. 

Have a quick browse on how this phase is defused successfully, we can find that there are many ways jumping to *line +123*, which compares the second input with `%eax`. What's more, there are many lines moving immediate number to `%eax` like *line +57, +64*, etc. We can guess the interesting *line +50* is to jump to one of the line. 

After trying `(gdb) x/w 0x402470: 0x00400f7c`, it refers to *line +57*, which means the base of the jumping is *line +57*. So, one of the answers: **1 311** comes out naturally.

