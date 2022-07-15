---
layout: default
---

[/index](../bios.md)

Learn how to code basic stuff (loops, functions) with the assembly langauge

Strings
-------

```nasm
mystring:
    db 'Hello, World', 0
```

Notice that text surrounded with quotes is converted to ASCII by the assembler,
while that lone zero will be passed as byte `0x00` (null byte)


Control structures
------------------

We have already used one: `jmp $` for the infinite loop.

Assembly jumps are defined by the *previous* instruction result. For example:

```nasm
cmp ax, 4      ; if ax = 4
je ax_is_four  ; do something (by jumping to that label)
jmp else       ; else, do another thing
jmp endif      ; finally, resume the normal flow

ax_is_four:
    .....
    jmp endif

else:
    .....
    jmp endif  ; not actually necessary but printed here for completeness

endif:
```

There are many `jmp` conditions: if equal, if less than, etc. 

Calling functions
-----------------

As you may suppose, calling a function is just a jump to a label.

The tricky part are the parameters. There are two steps to working with parameters:

1. The programmer knows they share a specific register or memory address
2. Write a bit more code and make function calls generic and without side effects

Step 1 is easy. Let's just agree that we will use `ax` for the parameters.

```nasm
mov al, 'X'
jmp print
endprint:

...

print:
    mov ah, 0x0e  ; tty code
    int 0x10      ; I assume that 'al' already has the character
    jmp endprint  ; this label is also pre-agreed
```

The current `print` function will only return to `endprint`. What if some other function wants to call it? We are killing code reusage.

The correct solution offers two improvements:

- We will store the return address so that it may vary
- We will save the current registers to allow subfunctions to modify them without any side effects

To store the return address, the CPU will help us. Instead of using a couple of `jmp` to call subroutines, use `call` and `ret`.

To save the register data, there is also a special command which uses the stack: `pusha` and `popa`, which pushes all registers to the stack automatically and recovers them afterwards.


Including external files
------------------------

The syntax is
```nasm
%include "file.asm"
```
------
