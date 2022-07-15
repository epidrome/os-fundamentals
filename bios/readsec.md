---
layout: default
---

[/index](../bios)

Let the bootsector load data from disk in order to boot the kernel

Our OS won't fit inside the bootsector 512 bytes, so we need to read data from a disk in order to run the kernel.

Thankfully, we don't have to deal with turning spinning platters on and off, we can just call some BIOS routines, like we did to print characters on the screen.
To do so, we set `al` to `0x02` (and other registers with the required cylinder, head and sector) and raise the interrupt `int 0x13`

On this lesson we will use for the first time the *carry bit*, which is an extra bit present on each register which stores when an operation has overflowed its current capacity:

```nasm
mov ax, 0xFFFF
add ax, 1 ; ax = 0x0000 and carry = 1
```

The carry isn't accessed directly but used as a control structure by other operators, like `jc` (jump if the carry bit is set)

The BIOS also sets `al` to the number of sectors read, so always compare it to the expected number.


Code
----

The boot sector is actually sector 1 (the first one, sectors start at 1) of cylinder 0 of head 0 of hdd 0.

Thus, any bytes after byte 512 correspond to sector 2 of cylinder 0 of head 0 of hdd 0

The main routine will fill it with sample data and then let the bootsector read it.

```nasm
[org 0x7c00]
    mov bp, 0x8000 ; set the stack safely away from us
    mov sp, bp

    mov bx, 0x9000 ; es:bx = 0x0000:0x9000 = 0x09000
    mov dh, 2 ; read 2 sectors
    ; the bios sets 'dl' for our boot disk number
    
    call disk_load

    mov dx, [0x9000] ; retrieve the first loaded word, 0xdada
    call print_hex

    call print_nl

    mov dx, [0x9000 + 512] ; first word from second loaded sector, 0xface
    call print_hex

    jmp $

disk_load:
    pusha
    ; reading from disk requires setting specific values in all registers
    ; so we will overwrite our input parameters from 'dx'. Let's save it
    ; to the stack for later use.
    push dx

    mov ah, 0x02 ; ah <- int 0x13 function. 0x02 = 'read'
    mov al, dh   ; al <- number of sectors to read (0x01 .. 0x80)
    mov cl, 0x02 ; cl <- sector (0x01 .. 0x11)
                 ; 0x01 is our boot sector, 0x02 is the first 'available' sector
    mov ch, 0x00 ; ch <- cylinder (0x0 .. 0x3FF, upper 2 bits in 'cl')
    ; dl <- drive number. Our caller sets it as a parameter and gets it from BIOS
    ; (0 = floppy, 1 = floppy2, 0x80 = hdd, 0x81 = hdd2)
    mov dh, 0x00 ; dh <- head number (0x0 .. 0xF)

    ; [es:bx] <- pointer to buffer where the data will be stored
    ; caller sets it up for us, and it is actually the standard location for int 13h
    int 0x13      ; BIOS interrupt
    jc disk_error ; if error (stored in the carry bit)

    pop dx
    cmp al, dh    ; BIOS also sets 'al' to the # of sectors read. Compare it.
    jne sectors_error
    popa
    ret

; Magic number
times 510 - ($-$$) db 0
dw 0xaa55

; boot sector = sector 1 of cyl 0 of head 0 of hdd 0
; from now on = sector 2 ...
times 256 dw 0xdada ; sector 2 = 512 bytes
times 256 dw 0xface ; sector 3 = 512 bytes

```

**Note: if you keep getting errors and your code seems fine, make sure that qemu is booting from the right drive**

There are two quick options:

1. Try the flag `-fda` for example, `qemu-system-x86_64 -fda binfile` which will set `dl`
as `0x00`, it seems to work fine then.
2. Explicitly use the flag `-boot`, e.g. `qemu-system-x86_64 binfile -boot c` which automatically sets `dl` as `0x80` and lets the bootloader read data

