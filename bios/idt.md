---
layout: default
---

[/index](../bios.md)

To better understand and keep track of the content check the provided code on [moyaOS](https://github.com/moya10/moyaOS/tree/master/CPU) repository.

Interrupts
----------

Interrupts are one of the main things that a kernel needs to handle. We will implement it as to be able to receive keyboard input.

Another examples of interrupts are: divisions by zero, out of bounds, invalid opcodes, page faults, etc.

Interrupts are handled on a vector, with entries which are similar to those of the GDT. However, instead of programming the IDT in assembly, we'll do it in C.

`cpu/idt.h` defines how an idt entry is stored `idt_gate` (there need to be
256 of them, even if null, or the CPU may panic) and the actual idt structure that the BIOS will load, `idt_register` which is just a memory address and a size, similar to the GDT register.

Finally, we define a couple variables to access those data structures from assembler code.

`cpu/idt.c` just fills in every struct with a handler. As you can see, it is a matter
of setting the struct values and calling the `lidt` assembler command.

ISRs
----

The Interrupt Service Routines run every time the CPU detects an interrupt, which is usually fatal. 

We will write just enough code to handle them, print an error message, and halt the CPU.

On `cpu/isr.h` we define 32 of them, manually. They are declared as `extern` because they will be implemented in assembler, in `cpu/interrupt.asm`

Before jumping to the assembler code, check out `cpu/isr.c`. As you can see,
we define a function to install all isrs at once and load the IDT, a list of error
messages, and the high level handler, which kprints some information. You
can customize `isr_handler` to print/do whatever you want.

Now to the low level which glues every `idt_gate` with its low-level and
high-level handler. Open `cpu/interrupt.asm`. Here we define a common
low level ISR code, which basically saves/restores the state and calls
the C code, and then the actual ISR assembler functions which are referenced
on `cpu/isr.h`

Note how the `registers_t` struct is a representation of all the registers
we pushed in `interrupt.asm`.

IRQ handlers
----

When the CPU boots, the PIC (programmable interrupt controller) maps IRQs 0-7 to INT 0x8-0xF and IRQs 8-15 to INT 0x70-0x77. This conflicts with the ISRs we programmed previously. Since we programmed ISRs 0-31, it is standard to remap the IRQs to ISRs 32-47.

The PICs are communicated with via I/O ports. The Master PIC has command 0x20 and data 0x21, while the Slave has command 0xA0 and data 0xA1.

The code for remapping the PICs is weird and includes some masks, so check 
[this article](http://www.osdev.org/wiki/PIC) if you're curious. Otherwise, just look at `cpu/isr.c`, new code after we set the IDT
gates for the ISRs. After that, we add the IDT gates for IRQs.

Now we jump to assembler, at `interrupt.asm`. The first task is to add global definitions for the IRQ symbols we just used in the C code. Look at the end of the `global` statements.

Then, add the IRQ handlers. Same `interrupt.asm`, at the bottom. Notice how we jump to a new common stub: `irq_common_stub` (next step)

We then create this `irq_common_stub` which is very similar to the ISR one. It is located at the top of `interrupt.asm`, and it also defines a new `[extern irq_handler]`

Now back to C code, to write the `irq_handler()` in `isr.c`. It sends some EOIs to the PICs and calls the appropriate handler, which is stored in an array named `interrupt_handlers` and defined at the top of the file. The new structs are defined in `isr.h`. We will also use a simple function to register the interrupt handlers.