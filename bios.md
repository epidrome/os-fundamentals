---
layout: default
---
[/index](./)

![](./assets/images/os-definition-and-functions.png)

A kernel can be defined as an interface between user and hardware. It is responsible for the execution of all the processes, Resource Allocation, CPU management, File Management and many other tasks.

The purpose of an operating system is to provide an environment in which a user can execute programs in convenient and efficient manner.

![](./assets/images/structur-of-a-computer-system.png)
Structure of a Computer System
A Computer System consists of:

- Users (people who are using the computer)
- Application Programs (Compilers, Databases, Games, Video player, Browsers, etc.)
- System Programs (Shells, Editors, Compilers, etc.)
- Kernel ( A special program which acts as an interface between user and hardware )
- Hardware ( CPU, Disks, Memory, etc)

### tutorial guide

[setup an environment](/bios/intro.md)
[boot sector](/bios/bootsec.md)
[printing on screen](/bios/printsec.md)
[code loops, functions](/bios/codesec.md)
[address memory in 16-bit real mode segmentation](/bios/addressec.md)
[read data from disk](/bios/readsec.md)
[define the GDT](/bios/gdt.md)
[enter 32-bit protected mode](/bios/pm.md)
[Create a development environment to build your kernel](/bios/env.md)
[create a simple kernel and a bootsector to load it](/bios/kernel.md)
[use VGA card data ports](/bios/vga.md)
[setup the IDT to handle CPU interrupts](/bios/idt.md)
[IRQ handlers, timer & keyboard](/bios/irq.md)
[parse user input - tiny shell](/bios/shell.md)
[memory allocator](/bios/alloc.md)