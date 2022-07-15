---
layout: default
---

[/index](../bios)

Make the boot sector print some text

In order to print something on the screen we will raise an interrupt.

On this example we are going to write each character of the "Hello" word into the register `al` (lower part of `ax`), the bytes `0x0e` into `ah` (the higher part of `ax`) and raise interrupt `0x10` which is a *general interrupt* for video services.

`0x0e` on `ah` tells the video interrupt that the actual function
we want to run is to 'write the contents of `al` in tty mode'.

Our new boot sector looks like this:
```nasm
mov ah, 0x0e ; tty mode
mov al, 'H'
int 0x10
mov al, 'e'
int 0x10
mov al, 'l'
int 0x10
int 0x10 ; 'l' is still on al, remember?
mov al, 'o'
int 0x10

jmp $ ; jump to current address = infinite loop

; padding and magic number
times 510 - ($-$$) db 0
dw 0xaa55 
```

You can examine the binary data with `xxd file.bin` on Linux envs.

As for later on ...

`nasm -f bin boot_sect_hello.asm -o boot_sect_hello.bin`

`qemu-system-x86_64 boot_sect_hello.bin`

